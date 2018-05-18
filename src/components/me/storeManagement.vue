<template>
    <div>
      <p class="storeWrap"><router-link to="/me"><img src="./meImgs/hose_back.png" alt=""></router-link> <span>仓库管理</span></p>
      <storeFooter></storeFooter>
    </div>
</template>

<script>
  import store from '@/vuex/index';
  import storeFooter from '@/components/me/storeManComFooter';
  export default {
    name: "login",
    created(){
      store.commit('footHide');
    },
    components: {storeFooter}
  }
</script>

<style scoped>
  .storeWrap {
    vertical-align: middle;
    background: #fff;
    padding: .3rem;
    text-align: left;
  }
  .storeWrap img {
    width: .6rem;
    padding-right: .3rem;
    margin-right: .2rem;
    border-right: 1px solid #ddd;
  }

</style>
