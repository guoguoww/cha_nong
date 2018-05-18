<template>
    <div>
      <p class="bindCardMC">
        <a href="javascript:;" @click="this.backTop">
          <div class="backIconC"><i></i></div>
        </a>
        <span>填写银行卡信息</span>
      </p>
      <p class="alertContent">为了保证资金安全，只能绑定认证用户本人的银行卡，暂不支持信用卡。</p>
      <div>
        <ul class="userBankInfo">
          <li><span>持卡人姓名</span><input type="text" placeholder="持卡人姓名" maxlength="10" v-model="cardName"></li>
          <li><span>身份证</span><input type="text" placeholder="身份证号" maxlength="18" v-model="idCode"></li>
          <li><span>银行卡号</span><input type="text" placeholder="银行卡号"  v-model="bankCard"></li>
          <li><span>开户行</span><input type="text" placeholder="填写开户银行" v-model="kaiHuBank"></li>
          <li><span>银行预留手机</span><span class="selTwoOne"></span></li>
          <li class="ylPhoneCodeC">
            <div class="ylPhoneTwoSel">
              <div><input type="radio" name="selXZ" checked ><span>使用注册手机号 </span> <span> {{ mobile}}</span>（不可修改）</div>
            </div>
          </li>
        </ul>
        <div class="bankBtn">
          <button @click="qeurenBank" v-show="initS">确认</button>
          <button @click="updateBankInfo" v-show="!initS">修改信息</button>
        </div>
      </div>
    </div>
</template>

<script>
  import Vue from 'vue';
  import {Toast} from 'vant';
  Vue.use(Toast);
  import store from '@/vuex/index';
  import {isCardNo} from './valifyIDCard';
  export default {
    name: "card-management",
    data(){
      return{
        mobile: localStorage.getItem('userMobile'),
        cardName: '',//持卡人
        idCode: '',//身份证号
        bankCard: '',//银行卡号
        kaiHuBank: '',//开户行
        initS: true,
      }
    },
    created(){
      store.commit('footHide');
      //接收router跳转时带的参数
      this.$http({
        method: 'get',
        url: this.url + '/api/my/getBankCard',
        header: {
          'Content-type': 'application/x-www-form-urlencoded'
        }
      }).then(res => {
        if (res.data.error_code === 0){
          if (res.data.data.cardInfo === ''){//需要绑定银行卡的
            this.initS = true;
          }else {
            this.initS = false;
            this.cardName = res.data.data.cardInfo.chnName;
            this.idCode = res.data.data.cardInfo.idCard;
            this.bankCard = res.data.data.cardInfo.bankAccount;
            this.kaiHuBank = res.data.data.cardInfo.bankName;
          }
        }
      }).catch(error => {

      })
    },
    methods: {
      qeurenBank(){
        //首先要先做各个输入框的验证，验证成功后再发请求
        var reg = /^[\u4e00-\u9fa5]{2,4}$/i;//身份证姓名的验证只能是2-4个汉字！
        if (!reg.test(this.cardName)){
          Toast('请输入真实姓名，只能是2-4个汉字！');
          return false;
        }else if(this.idCode == '') {
          Toast('身份证不能为空');
          return false;
        }else if (!isCardNo(this.idCode)){
          Toast('您输入的身份证号码不正确，请重新输入');
          return false;
        }else if(this.bankCard == '' || this.bankCard == null){
          Toast('卡号不能为空');
          return false;
        }else if(this.kaiHuBank == null || this.kaiHuBank == ''){
          Toast('开户行不能为空');
          return false;
        }else{
          this.$http({
            method: 'post',
            url: this.url + '/api/my/addBankCard',
            header: {
              'Content-type': 'application/x-www-form-urlencoded'
            },
            params: {
              'chnName': this.cardName,//持卡人
              'bankAccount': this.bankCard,//卡号
              'bankName': this.kaiHuBank,//开户行
              'phone': this.mobile,//手机号码
              'idCard': this.idCode,//身份证号码
            }
          }).then(res => {
            if(res.data.error_code === 0){
              Toast('银行卡绑定成功');
              this.$router.push({
                path: '/me'
              })
            }
          }).catch(error => {
            ;
          })
        }
      },
      updateBankInfo(){
        //首先要先做各个输入框的验证，验证成功后再发请求
        var reg = /^[\u4e00-\u9fa5]{2,4}$/i;//身份证姓名的验证只能是2-4个汉字！
        if (!reg.test(this.cardName)){
          Toast('请输入真实姓名，只能是2-4个汉字！');
          return false;
        }else if(this.idCode == '') {
          Toast('身份证不能为空');
          return false;
        }else if (!isCardNo(this.idCode)){
          Toast('您输入的身份证号码不正确，请重新输入');
          return false;
        }else if(this.bankCard == '' || this.bankCard == null){
          Toast('卡号不能为空');
          return false;
        }else if(this.kaiHuBank == null || this.kaiHuBank == ''){
          Toast('开户行不能为空');
          return false;
        }else{
          this.$http({
            method: 'post',
            url: this.url + '/api/my/updateBankCard',
            header: {
              'Content-type': 'application/x-www-form-urlencoded'
            },
            params: {
              'chnName': this.cardName,//持卡人
              'bankAccount': this.bankCard,//卡号
              'bankName': this.kaiHuBank,//开户行
              'phone': this.mobile,//手机号码
              'idCard': this.idCode,//身份证号码
            }
          }).then(res => {
            if(res.data.error_code === 0){
              Toast('银行卡修改成功');
              this.$router.push({
                path: '/me'
              })
            }
          }).catch(error => {
            ;
          })
        }

      },
      validForm(){
        //首先要先做各个输入框的验证，验证成功后再发请求
        var reg = /^[\u4e00-\u9fa5]{2,4}$/i;//身份证姓名的验证只能是2-4个汉字！
        if (!reg.test(this.cardName)){
          Toast('请输入真实姓名，只能是2-4个汉字！');
          return false;
        }else if(this.idCode == '') {
          Toast('身份证不能为空');
          return false;
        }else if (!isCardNo(this.idCode)){
          Toast('您输入的身份证号码不正确，请重新输入');
          return false;
        }else if(this.bankCard == '' || this.bankCard == null){
          Toast('卡号不能为空');
          return false;
        }else if(this.kaiHuBank == null || this.kaiHuBank == ''){
          Toast('开户行不能为空');
          return false;
        }
      },
    }
  }
</script>

<style scoped>
  .bindCardMC {
    background: #974f02;
    color: #fff;
    text-align: center;
    position: relative;
    padding: .4rem;
    font-size: .45rem;
  }
  .backIconC {
    position: absolute;
    top: .3rem;
    left: .2rem;
    color: #fff;
  }
  .backIconC i {
    display: inline-block;
    width: .3rem;
    height: .3rem;
    border-top: 1px solid #fff;
    border-left: 1px solid #fff;
    transform: rotate(-45deg);
    margin-right: .1rem;
  }
  .alertContent {
    background: yellow;
    color: red;
    padding: .1rem;
    font-size: .2rem;
  }
  .userBankInfo {
    padding: .1rem .2rem;
  }
  .userBankInfo li {
    display: flex;
    justify-content: flex-start;
    padding: .25rem 0;
    border-bottom: 1px solid #dddddd;
    text-align: left;
  }
  .userBankInfo li > span {
    width: 25%;
  }
  .selTwoOne {
    display: inline-block;
    margin-left: .2rem;
    color: #dddddd;
  }
  .bankBtn {
    width: 20%;
    margin: 1rem auto;
  }
  .ylPhoneCodeC {
    margin-left: .8rem;
    display: block !important;
    font-size: .35rem;
  }
  .ylPhoneTwoSel > div{
    display: flex;
    justify-content: flex-start;
    border-bottom: 1px solid  #dddddd;
    padding: .2rem;
  }
  .ylPhoneTwoSel > div input:first-child {
    width: .5rem;
    vertical-align: bottom;
    margin-right: .3rem;
  }
  .bankBtn button {
    width: 100%;
    background: #974f02;
    color: #fff;
    text-align: center;
    border-radius: .5rem;
    padding: .2rem;
  }

  .srYZCode input {
    width: 2.5rem !important;
    margin-left: .8rem;
    padding: .1rem;
  }
</style>
