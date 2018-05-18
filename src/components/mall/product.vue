<template>
  <div class="pro_wrap">
    <div v-if="curs" class="buy">
      <p>您选购了1件商品</p>
      <div class="buyChild">
        <div class="allNumC">
          <img src="./mallImgs/product_all_num.png" alt="">
          数量：<span class="totalN">{{num}}</span>
        </div>
        <div class="totalM">
          <img src="./mallImgs/product_all_money.png" alt="">
          总金额：<span class="allMoney">{{num*dPrice}}</span>
        </div>
        <div class="buyBtn">
          <img src="./mallImgs/product_buy_all.png"  @click="buy">
        </div>
      </div>
    </div>
    <div class="hidProInfo" v-show="currentState">

      <div class="hidHeader">
        <img src="./mallImgs/1511172136562.jpg" >
        <div>
          <p>{{proname}}</p>

          <p>总计金额：<span class="totalMoney">{{num*dPrice}}</span></p>
        </div>
        <div class="closeIcon"><img src="./mallImgs/icon_close.png" alt="" @click="closeCur"></div>
      </div>
      <ul class="hidUlList">
        <li><span>数量：</span><span>{{num}}</span></li>
        <li><span>单价：</span><span>{{dPrice}}</span></li>
        <li><span>手续费：</span><span>{{subFeeScale}}</span></li>
      </ul>
      <button type="button" class="qrBtnBuy" @click="qurenBuyPro()">确认购买</button>
    </div>
    <div class="hidShadowC" v-show="currentState"></div>
  </div>
</template>

<script>
    import Vue from 'vue';
    import {Toast} from 'vant';
    Vue.use(Toast);
    export default {
        name: "product",
        data(){
          return {
            active: false,
            currentState: false,
            subFeeScale: '',
          }
        },
        props: ['curs','num','dPrice','proname','proid','gdproid','gdmobile','proCurPrice'],
        methods: {
          buy(){
            //要先判断是否登录了
            let token = localStorage.getItem('value');
            if(token){//如果是已经登录的状态了
              this.currentState = true;
              //调取服务费的接口
              this.$http({
                method: 'get',
                url: this.url + '/api/sub/getScales',
                header: {
                  'Content-type': 'application/x-www-form-urlencoded'
                }
              }).then(res => {
                if (res.data.error_code ===0 ){
                  this.subFeeScale = (res.data.data.scalesBean.subFeeScale*(this.num*this.dPrice)).toFixed(2);
                }
              }).catch(error => {
                ;
              })
            }else {
              this.$router.push({
                path: '/login'
              })
            }
          },
          closeCur(){
            this.currentState = false;
          },
          qurenBuyPro(){
            //当点击确认购买的时候，要先判断 账户剩余货款是否充足
            this.$http({
              method: 'post',
              url: this.url + '/api/sub/buyHangGoods',
              header: {
                'Content-type': 'application/x-www-form-urlencoded'
              },
              params: {
                'goodsId': this.proid,//商品id
                'goodsName': this.proname,
                'holdNum': this.num,
                'hangUserPhone': this.gdmobile,
                'curPrice': this.proCurPrice,//商品当前价格
                'hangOrderId': this.gdproid,//挂单商品id
                'feeBuy': this.subFeeScale,
                'buyMoney': this.num*this.dPrice,
              }
            }).then(res=>{
              if(res.data.error_code===0){
                Toast('购买产品成功');
                this.$router.push({
                  path: '/orderManagement',
                })
              }
            }).catch(error=>{
              ;
            })
          }
        }
    }
</script>

<style scoped>
  .pro_wrap {
  }
  .pro_list_content li {
    background: #fff;
    width: 49.5%;
    margin-top: .3rem;
  }

  .pro_detail dt {
    font-weight: bold;
  }
  .pro_detail dd {
    font-size: .3rem;
  }
  .pro_detail dd.pro_uname i{
    background: url("./mallImgs/product_name.png") center center no-repeat;
    background-size: contain;
  }
  .pro_detail dd.pro_num i{
    background: url("./mallImgs/product_num.png") center center no-repeat;
    background-size: contain;
  }
  .pro_detail dd.pro_price i{
    background: url("./mallImgs/product_price.png") center center no-repeat;
    background-size: contain;
  }
  .pro_detail dd i {
    width: .35rem;
    height: .35rem;
    display: inline-block;
    margin-right: .2rem;
  }

  .buy{
    position: fixed;
    bottom: 1.3rem;
    width: 100%;
    left: 0;
    background: #fff;
    text-align: left;
    padding: .2rem .3rem;
    border-top: 1px solid #ddd;
    font-size: .3rem;
  }
  .buyChild {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .allNumC img,.totalM img {
    width: .35rem;
    margin-right: .1rem;
  }
  .buyBtn img {
    width: 3rem;
  }
  .hidProInfo {
    background: #fff;
    position: fixed;
    bottom: 0;
    z-index: 9999;
    left: 0;
    width: 100%;
  }
  .totalMoney {
    font-size: .5rem;
    color: #e21918;
  }
  .hidHeader {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 0 .5rem;
    position: relative;
  }
  .hidHeader img {
    width: 3rem;
    margin-top: -.3rem;
    margin-right: .5rem;
  }
  .hidHeader > div {
    line-height: 2;
    text-align: left;
  }
  .hidUlList {
    text-align: left;
    padding: .5rem;
    line-height: 2;
  }
  .qrBtnBuy {
    font-size: .5rem;
    background: #178f4a;
    color: #fff;
    width: 100%;
    padding: .3rem 0 ;
  }
  .closeIcon {
    position: absolute;
    top: .2rem;
    right: 0;
  }
  .closeIcon img {
    width: .5rem;
  }
  .hidShadowC {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.5);
  }
</style>
