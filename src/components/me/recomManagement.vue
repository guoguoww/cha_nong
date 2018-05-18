<template>
    <div>
      <p class="commandGLC"><router-link to="/me"><img src="./meImgs/hose_back.png" alt=""></router-link><span>推荐管理</span></p>
      <!--<p class="dataUpdate">数据更新时间：<span>2018-04-20 00:00:00</span></p>-->
      <ul class="comListTitle">
        <li>
          <span>姓名</span>
          <span>手机</span>
          <span>推广数量合计</span>
          <!--<span>有效推广数量</span>-->
        </li>
      </ul>
      <ul class="recomContentInfo">
        <li v-for="recomItem of recommandList">
          <span>{{recomItem.chnName.substring(0,3)}}****{{recomItem.chnName.substring(7,11)}}</span>
          <span>{{recomItem.mobile.substring(0,3)}}****{{recomItem.mobile.substring(7,11)}}</span>
          <span>{{recomItem.parent2Total}}</span>
          <!--<span>{{recomItem.parent2Total}}</span>-->
        </li>
      </ul>
    </div>
</template>

<script>
  import Vue from 'vue';
  import {Toast} from 'vant';
  Vue.use(Toast);
  import store from '@/vuex/index';
  export default {
    name: "recom-management",
    data(){
      return {
        recommandList: [],
      }
    },
    created(){
      store.commit('footHide');
      this.$http({
        method: 'post',
        url: this.url + '/api/my/corps',
        header: {
          'Content-type': 'application/x-www-form-urlencoded'
        }
      }).then(res=> {
        if (res.data.error_code ===0){
          this.recommandList = res.data.data.corps;
        }
      }).catch(error => {
        ;
      })
    }
  }
</script>

<style scoped>
  .commandGLC {
    background: #fff;
    padding: .3rem;
    text-align: left;
  }
  .commandGLC img {
    width: .6rem;
    padding-right: .3rem;
    margin-right: .2rem;
    border-right: 1px solid #ddd;
    vertical-align: middle;
  }
  .dataUpdate {
    background: yellow;
    padding: .1rem .5rem;
    text-align: left;
  }
  .comListTitle {
    background: #eeeeee;
    padding: .3rem .6rem;
  }
  .comListTitle li {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .comListTitle li span{
    width: 20%;
    text-align: left;
  }
  .comListTitle li span:nth-child(2),.comListTitle li span:nth-child(3){
    width: 30%;
  }
  .recomContentInfo {
    padding: .3rem .6rem;
    background: #fff;
  }
  .recomContentInfo li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: .1rem 0;
  }
  .recomContentInfo li span{
    width: 20%;
  }
  .recomContentInfo li span:first-child{
    width: 30%;
    text-align: left;
  }
  .recomContentInfo li span:last-child{
     width: 30%;
     text-align: right;
   }
</style>
