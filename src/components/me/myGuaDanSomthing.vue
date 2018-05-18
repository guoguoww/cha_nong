<template>
    <div>
      <ul class="gdProTitle">
        <li>商品名</li>
        <li>挂单数量</li>
        <li>买入价</li>
        <li>挂单时间</li>
      </ul>
      <ul class="gdProDetaInfos">
        <li v-for="(item,index) of gdProInfos">
          <div class="curDetaiContent" @click="changeState(index)">
            <span>{{item.goodsName}}</span>
            <span>{{item.holdNum}}</span>
            <span>{{item.buyPoint}}</span>
            <span>{{item.hangTime}}</span>
          </div>
          <ul class="curHideInfos" v-show="initState == index">
            <li>
              <p><span>流水号:</span><span>{{item.serialNo}}</span></p>
              <p><span>买入时间:</span><span>{{item.buyTime}}</span></p>
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
      name: "my-gua-dan-something",
      data(){
        return{
          gdProInfos:[],
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
            'status':2,
          }
        }).then(res => {
          if (res.data.error_code===0) {
            this.gdProInfos = res.data.data.list;
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
  .gdProTitle {
    padding: .3rem 0 ;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .gdProTitle li {
    width: 25%;
  }
  .gdProDetaInfos li div{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: .2rem 0;
  }
  .gdProDetaInfos li span {
    width: 25%;
  }
  .gdProDetaInfos li:first-child {
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
  .curDetaiContent span{
    color: #e21918;
  }
</style>
