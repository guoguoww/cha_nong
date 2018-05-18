<template>
    <div>
      <ul class="proTitle" >
        <li>商品名</li>
        <li>数量</li>
        <li>总额</li>
        <li>服务费</li>
        <li>手续费</li>
        <li>单价</li>
      </ul>
      <ul class="proDetaInfos" >
        <li v-for="(item,index) of proInfos" >
          <div class="curDetaiContent" @click="changeState(index)">
            <span>{{item.goodsName}}</span>
            <span>{{item.holdNum}}</span>
            <span>{{item.money}}</span>
            <span>{{item.serviceFee}}</span>
            <span>{{item.feeBuy}}</span>
            <span>{{item.buyPoint}}</span>
          </div>
          <ul class="curHideInfos" v-show="initState == index">
            <li>
              <p><span>流水号:</span><span>{{item.serialNo}}</span></p>
              <p><span>买入时间:</span><span>{{item.buyTime}}</span></p>
            </li>
            <li class="gdNumContent">
              <span>挂单数量:</span>
              <input type="number" placeholder="请输入挂单数量" v-model="hangNumber">
              <!--<button class="gdBtn" @click="hangOrder(item.id)">挂单</button>-->
              <button class="gdBtn" @click="devIng">挂单</button>
            </li>
          </ul>
        </li>
      </ul>
    </div>
</template>

<script>
    export default {
      name: "my-buy-somthing",
      created(){
        this.$http({
          method: 'get',
          url: this.url + '/api/my/getMyTrade',
          header: {
            'Content-type': 'application/x-www-form-urlencoded'
          },
          params: {
            'status':1,
          }
        }).then(res => {
          if (res.data.error_code===0) {
            this.proInfos = res.data.data.list;
          }
        }).catch(error => {
          ;
        })
      },
      data(){
        return {
          proInfos: [],
          initState: -1,
          hangNumber: '',
        }
      },
      methods: {
        devIng(){
          this.$Toast('正在开发中...');
        },
        changeState(index){
          this.initState = this.initState == index ? -1 : index;
        },
        hangOrder(proId){
          if (this.hangNumber){
            this.$http({
              method:'post',
              url: this.url + '/api/trade/entryOrders',
              header: {
                'Content-type': 'application/x-www-form-urlencoded'
              },
              params:{
                'id': proId,
                'holdNum': this.hangNumber
              }
            }).then(res => {
              if (res.data.error_code ===0){
                this.$Toast('挂单成功');
                this.initState = -1;
                this.hangNumber = '';
                this.$http({
                  method: 'get',
                  url: this.url + '/api/my/getMyTrade',
                  header: {
                    'Content-type': 'application/x-www-form-urlencoded'
                  },
                  params: {
                    'status':1,
                  }
                }).then(res => {
                  if (res.data.error_code===0) {
                    this.proInfos = res.data.data.list;
                  }
                }).catch(error => {
                  ;
                })
              }else {
                this.$Toast(res.data.data.error);
              }
            }).catch(error => {
              ;
            })
          }else {
            this.$Toast('挂单数量不能为空');
          }
        },
      }
    }
</script>

<style scoped>
  .proTitle {
    padding: .3rem 0 ;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .proTitle li {
    width: 16%;
  }
  .proDetaInfos > li > div{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: .2rem 0;
  }
  .proDetaInfos li div span {
    width: 16%;
  }
  .proDetaInfos li:first-child {
    padding-top: 0;
  }
  .curHideInfos {
    padding: 0 .3rem;
    background: #dddddd;
   }
  .curHideInfos li.gdNumContent {
    padding: .1rem;
    display: flex;
  }
  .gdNumContent span{
    width: 20%;
  }
  .gdNumContent input {
    width: 60%;
  }
  .gdNumContent button {
    width: 20%;
  }
  .curHideInfos li {
    text-align: left;
    margin-bottom: .1rem;
  }
  .gdBtn {
    background: #974f02;
    padding: .1rem .2rem;
    border-radius: .2rem;
    border: none;
    color: #fff;
  }
  .curDetaiContent span{
    color: #e21918;
  }
</style>
