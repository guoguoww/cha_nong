<template>
    <div>
      <div class="account_set"><p><router-link to="/me"><img src="./meImgs/set_back.png" alt=""></router-link> <span>设置</span></p>
        <!--<span class="saveN" v-if="seen" @click="saveAgain()">保存</span>-->
      </div>
      <ul class="account_user_info">
        <li>
          <span>昵称</span>
          <div class="nicknameW">
            <input class="nickname" type="text" disabled v-model="userInfo.serialNo" >
            <i class="arrowRight"></i>
          </div>
        </li>
        <li>
          <span>姓名</span>
          <div class="nicknameW">
            <input class="nickname" type="text" disabled v-model="xname">
            <i class="arrowRight"></i>
          </div>
        </li>
        <li>
          <span>手机号</span>
          <div class="nicknameW">
            <input class="nickname" type="text" disabled v-model="mobileN">
            <i class="arrowRight"></i>
          </div>
        </li>
      </ul>
      <ul class="accountMoney">
        <!--<li><span>资金密码</span><i class="arrowRight"></i></li>-->
        <li><router-link to="/modifyPwd"><span>修改登录密码</span><i class="arrowRight"></i></router-link></li>
        <!--<li><router-link :to="{path:'/forgetPwd' , query:{data: mobileN }}"><span>忘记资金密码</span><i class="arrowRight"></i></router-link></li>-->
      </ul>
      <ul class="aboutUs">
        <li @click="aboutUs"><span>关于我们</span><span>v1.1.7</span></li>
      </ul>
    </div>
</template>

<script>
    import Vue from 'vue';
    import { Toast } from 'vant';
    Vue.use(Toast);
    export default {
      name: "account-set",
      data(){
        return {
          userInfo: {},
          xname: '',//用户姓名
          mobileN: localStorage.getItem('userMobile'),
        }
      },
      created(){
        // 接收me组件中传递过来的数据
        // 取到路由带过来的参数
        this.userInfo = this.$route.query.data;

      },
      methods: {
        aboutUs(){
          Toast('努力开发中，请耐心等待哦~')
        },
      }
    }
</script>

<style scoped>
  .account_set {
    background: #666;
    padding: .3rem;
    color: #fff;
    text-align: left;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .account_set img {
    width: .35rem;
    margin-right: .4rem;
  }
  .account_user_info {
    margin-top: .2rem;
    background: #fff;
    padding: .2rem;
  }
  .account_user_info li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid #eee;
    padding: .25rem 0;
  }
  .nicknameW {
    width: 40%;
  }
  .account_user_info li div input {
    width: 80%;
    text-align: right;
  }
  .arrowRight {
    display: inline-block;
    width: .3rem;
    height: .3rem;
    border-top: 1px solid #aaa;
    border-right: 1px solid #aaa;
    transform: rotate(45deg);
    margin-top: .2rem;
  }
  .accountMoney {
    margin: .2rem 0;
    padding: .2rem;
    background: #fff;
    padding-right: .3rem;
  }
  .accountMoney li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid #eee;
    padding: .25rem 0;
  }
  .accountMoney li a{
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .aboutUs {
    margin: .2rem 0;
    padding: .1rem;
    background: #fff;
  }
  .aboutUs li{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: .25rem .1rem;
  }
  ul li:last-child {
    border-bottom: none;
  }
</style>
