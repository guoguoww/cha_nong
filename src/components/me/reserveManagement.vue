<template>
    <div>
      <div class="resManageWrap"><div><router-link to="/me"><img src="./meImgs/set_back.png" alt=""></router-link><span>预订管理</span></div><div><span>待支付预定</span></div></div>
    </div>
</template>

<script>
  import store from '@/vuex/index';
  export default {
    name: "reserve-management",
    created(){
      store.commit('footHide');
    }
  }
</script>

<style scoped>
  .resManageWrap {
    background: #e21918;
    padding: .3rem;
    color: #fff;
    vertical-align: middle;
    text-align: left;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .resManageWrap img {
    width: .6rem;
    padding-right: .3rem;
    margin-right: .2rem;
    border-right: 1px solid #ddd;
  }
</style>
