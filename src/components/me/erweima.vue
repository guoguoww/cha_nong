<template>
    <div>
      <p class="erweimaC"><router-link to="/me"><div class="erweimaIconC"><i></i></div></router-link><span>二维码分享</span></p>
      <div class="qrcodContent">
        <div id="qrcode">

        </div>
      </div>
    </div>
</template>

<script>
  import store from '../../vuex';
  import Vue from 'vue';
  import {Toast} from 'vant';
  import QRCode from 'qrcodejs2'
  Vue.use(Toast);
    export default {
      name: "erweima",
      data(){
        return {
          invitInfoId: '',
          invitorPhone: ''
        }
      },
      created(){
        store.commit('footHide');
        this.$http({
          method: 'get',
          url: this.url + '/api/user/getShareImg',
          header: {
            'Content-type': 'application/x-www-form-urlencoded'
          }
        }).then(res => {
          if (res.data.error_code === 0){
            //获取到二维码的参数
            this.invitInfoId = res.data.data.inviteId;
            this.invitorPhone = res.data.data.refereePhone;
            this.qrcode();
          }
        }).catch(error => {
          ;
        })
      },
      methods:{
        qrcode () {
          let qrcode = new QRCode(document.getElementById("qrcode"),
            {
              width: 180,
              height: 180,
              // 'text':"http://120.27.101.245/#/register?inviteId="+this.invitInfoId+"&refereePhone="+this.invitorPhone
              'text':"http://pay.pindao360.com/#/register?userId="+this.invitInfoId+"&refereePhone="+this.invitorPhone
              // 'text':"http://192.168.0.128:8888/#/register?inviteId="+this.invitInfoId+"&refereePhone="+this.invitorPhone

            });
        }
      }
    }
</script>

<style scoped>
  .erweimaC {
    background: #e07701;
    color: #fff;
    text-align: center;
    position: relative;
    padding: .3rem;
  }
  .erweimaIconC {
    position: absolute;
    top: .3rem;
    left: .2rem;
    color: #fff;
  }
  .erweimaIconC i {
    display: inline-block;
    width: .3rem;
    height: .3rem;
    border-top: 1px solid #fff;
    border-left: 1px solid #fff;
    transform: rotate(-45deg);
    margin-right: .1rem;
  }
  .qrcodContent {
    background: #e07701;
    text-align: center;
    padding: 1rem 0;
  }
  #qrcode {
    width: 50%;
    margin:auto;
  }

</style>
