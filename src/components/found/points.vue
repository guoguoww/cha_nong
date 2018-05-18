<template>
    <div>
      <p class="jifenMC"><router-link to="/found"><img src="./foundImgs/set_back.png" alt=""></router-link><span>积分明细</span></p>
      <div>
        <p class="pointsBg">
          <span>当前积分</span>
          <span class="scoreF">{{cruPoints}}</span>
          <span>分</span>
        </p>
      </div>
      <div>
        <ul class="pointDetailTitle">
          <li>
            <span>类型</span>
            <span>变化积分</span>
            <!--<span>变化前积分</span>-->
            <!--<span>变化后积分</span>-->
            <span>创建时间</span>
            <span>备注</span>
          </li>
        </ul>
        <ul class="pointDetailC">
          <li v-for="pointItem of pointDetailInfos">
            <span>{{pointItem.type}}</span>
            <span>{{pointItem.integral}}</span>
            <!--<span>{{pointItem.integralBefore}}</span>-->
            <!--<span>{{pointItem.integralAfter}}</span>-->
            <span>{{pointItem.createTime}}</span>
            <span>{{pointItem.remark}}</span>
          </li>
        </ul>
      </div>
    </div>
</template>

<script>
  import store from '../../vuex/';
  import Vue from 'vue';
  import {Toast} from 'vant';
  Vue.use(Toast);
    export default {
      name: "points",
      data(){
        return {
          pointDetailInfos: [],
          cruPoints: ''
        }
      },
      created(){
        store.commit('footHide');
        this.$http({
          method: 'get',
          url: this.url + '/api/sub/getUserIntegral',
          header: {
            'Content-type': 'application/x-www-form-urlencoded'
          },
        }).then(res => {
          if (res.data.error_code === 0){
            this.cruPoints = res.data.data.integral;
          }
        }).catch(error => {
          ;
        });


        this.$http({
          method: 'get',
          url: this.url + '/api/sub/getUserIntegralDetail',
          header: {
            'Content-type': 'application/x-www-form-urlencoded'
          },
        }).then(res => {
          if (res.data.error_code === 0){
            this.pointDetailInfos = res.data.data.integral;
          }
        }).catch(error => {
          ;
        })
      }
    }
</script>

<style scoped>
  .jifenMC {
    background: #2eae64;
    color: #fff;
    padding: .3rem;
    text-align: left;
  }
  .jifenMC img {
    width: .6rem;
    padding-right: .3rem;
    margin-right: .2rem;
    border-right: 1px solid #ddd;
    vertical-align: middle;
  }
  .pointsBg {
    background: url("./foundImgs/score_bg.png") center center no-repeat;
    background-size: contain;
    width: 40%;
    height: 3rem;
    margin: .5rem auto;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    color: #fff;
    padding: .4rem 0 ;
  }
  .scoreF {
    font-size: .7rem;
  }
  .pointDetailTitle li,.pointDetailC li {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .pointDetailTitle,.pointDetailC {
    padding: .2rem;
    margin-bottom: .1rem;
  }
  .pointDetailTitle li span {
    width: 25%;
  }
  .pointDetailTitle li span:first-child {
    width: 16%;
  }
  .pointDetailC li span {
    width: 20%;
    word-break: break-all;
  }
  .pointDetailC li span:nth-child(3){
    width: 30%;
  }
  .pointDetailC li span:nth-child(4){
    width: 30%;
  }
</style>
