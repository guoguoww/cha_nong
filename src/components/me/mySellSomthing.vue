<template>
    <div>
      <ul class="sellProTitle">
        <li>商品名</li>
        <li>商品数量</li>
        <li>总金额</li>
        <li>卖出手续费</li>
        <li>卖出单价</li>
        <li>盈亏金额</li>
      </ul>
      <ul class="sellProDetaInfos">
        <li v-for="(item,index) of sellProInfos">
          <div class="curDetaiContent" @click="changeState(index)">
            <span>{{item.goodsName}}</span>
            <span>{{item.holdNum}}</span>
            <span>{{item.money}}</span>
            <span>{{item.feeSell}}</span>
            <span>{{item.sellPoint}}</span>
            <span>{{item.difMoney}}</span>
          </div>
          <ul class="curHideInfos" v-show="initState == index">
            <li>
              <p><span>流水号：</span><span>{{item.serialNo}}</span></p>
              <p><span>服务费：</span><span>{{item.serviceFee}}</span></p>
            </li>
            <li>
              <p><span>买入时间：</span><span>{{item.buyTime}}</span></p>
              <p><span>买入手续费：</span><span>{{item.feeBuy}}</span></p>
            </li>
            <li>
              <p><span>卖出时间：</span><span>{{item.sellTime}}</span></p>
              <p><span>买入单价：</span><span>{{item.buyPoint}}</span></p>
            </li>
          </ul>
        </li>
      </ul>
    </div>
</template>

<script>
  import Vue from 'vue';
  import {Toast} from 'vant';
  Vue.use(Toast);
    export default {
      name: "my-sell-somthing",
      data(){
        return {
          sellProInfos: [],
          initState: -1,
        }
      },
      created(){
        this.$http({
          method: 'get',
          url: this.url + '/api/my/getMyTrade',
          header: {
            'Content-type': 'application/x-www-form-urlencoded'
          },
          params: {
            'status':3,
          }
        }).then(res => {
          if (res.data.error_code===0) {
            this.sellProInfos = res.data.data.list;
          }
        }).catch(error => {
          ;
        })
      },
      methods: {
        changeState(index){
          this.initState = this.initState == index ? -1 : index;
        },
      }
    }
</script>

<style scoped>
  .sellProTitle {
    padding: .3rem 0 ;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .sellProTitle li {
    width: 16%;
  }
  .sellProDetaInfos li div{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: .2rem 0;
  }
  .sellProDetaInfos li span {
    width: 16%;
  }
  .sellProDetaInfos li:first-child {
    padding-top: 0;
  }
  .curHideInfos {
    padding: 0 .3rem;
    background: #dddddd;
  }
  .curHideInfos li {
    padding: .1rem;
    text-align: left;
  }
  .curHideInfos li {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .curDetaiContent span{
    color: #e21918;
  }
</style>
