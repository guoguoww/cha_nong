<template>
  <div>
    <p class="moneyOutC"><router-link to="/me"><img src="./meImgs/set_back.png" alt=""></router-link><span>退货退款</span></p>
    <div class="moneyOutDetail">
      <p>
        <span>退款金额</span>
        <input type="text" placeholder="请输入退款金额" v-model="money"/>
      </p>
      <p>
        <span>退款密码</span>
        <input type="password" placeholder="请输入登录密码" v-model="password"/>
      </p>
    </div>
    <input class="moneyInBtn" type="button" value="确认退款" @click="moneyout"/>
  </div>
</template>

<script>
    import Vue from 'vue';
    import {Toast} from 'vant';
    Vue.use(Toast);
    export default {
        name: "money-out",
        data(){
          return {
            money: '',
            password: '',
            type: 0
          };
        },
        methods: {
          moneyout(){
            this.$http({
              method: 'post',
              url: this.url + '/api/moneyRecord/out',
              header: {
                'Content-type': 'application/x-www-form-urlencoded'
              },
              params: {
                money: this.money,
                password: this.password,
                type: 0
              }
            }).then(res=>{
              var data = res.data.data;
              if(res.data.error_code===0){
                  Toast('提现成功');
              }else {
                Toast(data.error);
              }
            }).catch(error=>{
              ;
            })
          }
        }
    }
</script>

<style scoped>
  .moneyOutC {
    background: #974f02;
    color: #fff;
    padding: .4rem;
    text-align: left;
    font-size: .45rem;
  }
  .moneyOutC img {
    width: .65rem;
    padding-right: .3rem;
    margin-right: .2rem;
  }
  .moneyOutDetail {
    margin-top: 1rem;
  }
  .moneyOutDetail p {
    display: flex;
    justify-content: space-around;
    align-items: center;
    margin-bottom: .3rem;
  }
  .moneyOutDetail p input {
    width: 70%;
    border: 1px solid #ddd;
    border-radius: .3rem;
    padding: .2rem;
  }
  .moneyInBtn {
    margin-top: .5rem;
    width: 50%;
    background: #974f02;
    color: #fff;
    border-radius: .5rem;
    padding: .2rem 0;
  }
</style>
