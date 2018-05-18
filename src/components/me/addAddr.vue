<template>
    <div class="add_address">
      <p class="add_addr_head"><router-link to="/addressC"><img src="./meImgs/hose_back.png" alt=""><span>添加地址</span></router-link></p>
      <addressCommonC></addressCommonC>
    </div>
</template>

<script>
  import store from '@/vuex/index';
  import addressCommonC from '@/components/me/addressCommonC';
  export default {
    name: "add_addr",
    created(){
      store.commit('footHide');
    },
    components:{addressCommonC}
  }
</script>

<style scoped>
  .add_addr_head {
    background: #fff;
    text-align: left;
    height: 1.3rem;
    line-height: 1.3rem;
    padding: 0 .3rem;
  }
  .add_addr_head img {
    width: .7rem;
    padding-right: .3rem;
    border-right: 1px solid #888;
    margin-right: .3rem;
    vertical-align: middle;
  }
  a {
    overflow: hidden;
  }
</style>
