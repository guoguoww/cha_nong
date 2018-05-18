<template>
    <div>
      <p class="zixunC">
        <a href="javascript:;" @click="this.backTop">
          <img src="./foundImgs/set_back.png" alt="">
        </a>
        <span class="pro_center_name">资讯</span>
      </p>
      <div>
        <div class="artTitle">
          <p class="artName">{{zxInfos.title}}</p>
          <p class="artDate">{{zxInfos.createTime}}</p>
        </div>
        <div class="artContent" v-html="zxContent"></div>
      </div>
    </div>
</template>

<script>
  import Vue from 'vue';
  import {Toast} from 'vant';
  Vue.use(Toast);
  import store from '../../vuex';
    export default {
      name: "zixun",
      data(){
        return {
          zxInfos: '',
          zxContent: ''
        }
      },
      created(){
        store.commit('footHide');
        //接收路由跳转带过来的参数
        this.zxInfos = JSON.parse(this.$route.query.data);
        console.log(this.$route.query.data);
        this.$http({
          method: 'get',
          url: this.url + '/api/index/getNewsById',
          header: {
            'Content-type': 'application/x-www-form-urlencoded',
          },
          params: {
            'id': this.zxInfos.id
          }
        }).then(res => {
          if (res.data.error_code === 0){
            this.zxContent = res.data.data.news.content;
          }
        }).catch(error => {

        })
      }
    }
</script>

<style scoped>
  .zixunC {
    background: #974f02;
    color: #fff;
    padding: .4rem;
    text-align: left;
    font-size: .45rem;
  }
  .zixunC img {
    width: .65rem;
    padding-right: .3rem;
    margin-right: .2rem;
  }
  .artTitle {
    background: #eee;
    text-align: center;
    line-height: 2;
    padding: .4rem 0;
  }
  .artDate {
    color: #bbb;
  }
  .artName {
    font-size: .45rem;
  }
  .artContent {
    padding: .2rem;
  }
</style>
