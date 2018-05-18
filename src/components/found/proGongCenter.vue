<template>
    <div>
      <header class="pro_gg_header">
        <router-link to="/found">
          <img src="./foundImgs/set_back.png" alt="">
        </router-link>
        <span class="pro_center_name">咨询中心</span>
      </header>
      <div class="pro_gg_detail">
        <foundCommon></foundCommon>
      </div>
    </div>
</template>

<script>
  import foundCommon from './foundCommon';
  import store from '../../vuex';
    export default {
      name: "proGongCenter",
      created(){
        store.commit('footHide');
      },
      components: {foundCommon},
    }
</script>

<style scoped>
  .pro_gg_header {
    background: #333;
    color: #fff;
    text-align: left;
    padding: .4rem ;
    font-size: .35rem;
    letter-spacing: 1px;
  }
  .pro_gg_header img {
    width: .6rem;
    padding-right: .25rem;
    border-right: 1px solid #fff;
    margin-right: .2rem;
  }
  .pro_gg_detail {
    padding: .3rem;
    background: #fff;
  }
</style>
