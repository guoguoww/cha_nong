<template>
    <div class="notice_wrap_c">
      <div class="notice_content">
        <span>通知公告:</span>
        <van-notice-bar mode="link" background="none" style="width: 80%">
        </van-notice-bar>
      </div>
    </div>

</template>

<script>
  import Vue from 'vue';
  import { NoticeBar } from 'vant';

  Vue.use(NoticeBar);

  export default {
    name: "notice-bar",

  }
</script>

<style scoped>
  .notice_wrap_c {
    border-bottom: 1px solid #ddd;
  }
  .notice_content {
    display: flex;
    justify-content: flex-start;
    padding: .1rem .4rem;
    padding-bottom: 0;
  }
  .notice_content span{
    padding-top: .2rem;
    text-align: left;
    color: #553009;
    font-weight: bold;
  }
</style>
