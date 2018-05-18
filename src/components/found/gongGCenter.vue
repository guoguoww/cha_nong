<template>
  <div>
    <header class="pro_gg_header">
      <a href="javascript:;" @click="this.backTop">
        <img src="./foundImgs/set_back.png" alt="">
      </a>
      <span class="pro_center_name">公告中心</span>
    </header>
    <div class="pro_gg_detail">
      <foundCommon></foundCommon>
    </div>
  </div>
</template>

<script>
  import foundCommon from './foundCommon';
  export default {
    name: "proGongCenter",
    components: {foundCommon}
  }
</script>

<style scoped>
  .pro_gg_header {
    background: #974f02;
    color: #fff;
    text-align: left;
    padding: .4rem ;
    font-size: .45rem;
    letter-spacing: 1px;
  }
  .pro_gg_header img {
    width: .65rem;
    padding-right: .25rem;
    margin-right: .2rem;
  }
  .pro_gg_detail {
    padding: .3rem;
    background: #fff;
  }
</style>
