<template>
    <div>
      <div>
        <p class="moneyMC">
          <a href="javascript:;" @click="this.backTop">
            <img src="./meImgs/arrow_left.png" alt="">
          </a>
          <span>我的资金</span>
        </p>
        <!--<ul class="moneyUserInfo">-->
          <!--<li>-->
            <!--<span>真实姓名</span>-->
            <!--<span>{{myInfos.mobile}}</span>-->
          <!--</li>-->
          <!--<li>-->
            <!--<span>手机号码</span>-->
            <!--<span>{{myInfos.mobile}}</span>-->
          <!--</li>-->
          <!--&lt;!&ndash;<li>&ndash;&gt;-->
            <!--&lt;!&ndash;<span>用户ID</span>&ndash;&gt;-->
            <!--&lt;!&ndash;<span>{{myInfos.id}}</span>&ndash;&gt;-->
          <!--&lt;!&ndash;</li>&ndash;&gt;-->
        <!--</ul>-->
        <!--<p class="syMoney">-->
          <!--<span>剩余货款 （元）</span>-->
          <!--<span>{{myInfos.money}}</span>-->
        <!--</p>-->
      </div>
      <div>
        <!--<p class="listTitle"><img src="./meImgs/moneym_list.png" alt=""> <span>流水单</span></p>-->
        <ul class="moneyListTab">
          <li @click="allOrderClick('allOrder')" :class="{active: (currentTab=='allOrder'?true:false)}">
            <p><img src="./meImgs/all_order.png" alt=""></p><span>全部订单</span>
          </li>
          <li @click="allOrderClick('yufuhuokuan')" :class="{active:(currentTab=='yufuhuokuan'?true:false)}">
            <p><img src="./meImgs/yufu_huokuan.png" alt=""></p><span>预付货款</span></li>
          <li @click="allOrderClick('tuihuotuikuan')" :class="{active:(currentTab=='tuihuotuikuan'?true:false)}">
            <p><img src="./meImgs/tuihuo_tuikuan.png" alt=""></p><span>退货退款</span></li>
        </ul>
      </div>
      <all-order :is="currentTab" keep-alive></all-order>
      <footer class="moneyMCFooter">
        <div>
          <span @click="moneyrecord">预付货款</span>
          <span>|</span>
          <span @click="moneyOut">货款退款</span>
        </div>
      </footer>
    </div>
</template>

<script>
  import store from '@/vuex/index';
  import Vue from 'vue';
  import {Toast} from 'vant';
  Vue.use(Toast);
  import yufuhuokuan from './moneyYuFuProMoney';
  import tuihuotuikuan from './backProbackMoney';
  import allOrder from './moneyAllOrders';

  export default {
    name: "money-management",
    data(){
      return{
        myInfos: '',
        active:true,
        currentTab: 'allOrder' // currentTab 用于标识当前触发的子组件
      }
    },
    created(){
      store.commit('footHide');
      //获取用户个人信息
      this.$http({
        method:'get',
        url: this.url + '/api/my/getMyInfo',
        header: {
          'Content-type': 'application/x-www-form-urlencoded'
        }
      }).then(res=>{  //请求成功
        if(res.data.error_code===0){  //返回成功
          this.myInfos=res.data.data.myInfo;
        }else{  //返回失败
          this.$router.push({
            path: '/login'
          });
          Toast(res.data.error_code);
        }
      }).catch(error=>{ //请求失败
        ;
      })
    },
    methods:{
      allOrderClick(tab){
        this.currentTab = tab; // tab 为当前触发标签页的组件名
      },
      moneyrecord(){
        this.$router.push({
          path: '/moneyRecord'
        });
      },
      moneyOut(){
        this.$router.push({
          path: '/moneyOut'
        });
      }
    },
    components: {
      allOrder,yufuhuokuan,tuihuotuikuan
    }
  }
</script>

<style scoped>
  .moneyMC {
    position: relative;
    background: #974f02;
    color: #fff;
    padding: .3rem;
    text-align: center;
    font-size: .5rem;
  }
  .moneyMC img {
    position: absolute;
    top: .4rem;
    left: .3rem;
    width: .3rem;
  }
  .moneyListTab li p img {
    width: .7rem;
  }
  .moneyUserInfo > li {
    display: flex;
    width: 33.3%;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
  }
  .moneyUserInfo li > span:first-child {
    margin-bottom: .1rem;
  }
  .syMoney {
    flex-direction: column;
  }
  .syMoney span:last-child {
    margin-top: .3rem;
    font-size: .7rem;
  }
  .listTitle {
    background: #fff;
    margin-top: .2rem;
    padding: .1rem;
    text-align: left;
  }
  .listTitle img {
    width: .5rem;
    margin-right: .1rem;
    padding-top: .1rem;
  }
  .moneyListTab {
    border-top: 1px solid #ddd;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    background: #fff;
  }
  .moneyListTab li {
    width: 33.33%;
    padding: .2rem;
  }
  .moneyListTab li.active {
    border-bottom: 1px solid #000;
  }

  .moneyMCFooter {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    background: #974f02;
    color: #fff;
    font-size: .45rem;
  }
  .moneyMCFooter > div {
    display: flex;
    justify-content: space-around;
    align-items: center;
    padding: .4rem 0;
  }
</style>
