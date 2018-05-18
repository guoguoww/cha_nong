<template>
  <div >
    <div class="mailheader">
      <select class="proTypeName"  v-model="morenVal"  @change="changePro">
        <option value="">全部商品</option>
        <option v-for="(item,index) of gxProInfos" :value="item.id">{{item.goodsName}}</option>
      </select>
    </div>
    <div style="margin-top: .4rem">
      <ul class="pro_list_content">
        <li style="position: relative" v-for="(ziItem,ziIndex) of proDetailInfos">
          <!--<router-link to="/productInfo">-->
            <div class="pro_img_content"><img :src="ziItem.imgLoadPath" alt=""></div>
            <dl class="pro_detail">
              <dt>{{ziItem.goodsName}}</dt>
              <dd class="pro_uname"><i></i><span>{{ziItem.mobile}}</span></dd>
              <dd class="pro_num"><i></i><span>数量:</span><span> {{ziItem.holdNum}}</span></dd>
              <dd class="pro_price"><i></i><span>单价:</span><span> {{getPrice}}</span> </dd>
            </dl>
          <!--</router-link>-->
          <span id="sel_circle" @click="checkCur(ziIndex,ziItem)" :class="{active: active == ziIndex}"></span>
        </li>
      </ul>
    </div>
    <pro_list :curs = 'initState' v-bind="cdProInfos"></pro_list>
  </div>
</template>

<script>
  import store from '@/vuex/index';//引入vuex状态管理器
  import Vue from 'vue';
  import {Toast} from 'vant';
  Vue.use(Toast);
  import pro_list from './product';
    export default {
      name: "mall",
      data(){
        return {
          active: -1,
          initState: false,
          gxProInfos: [],
          morenVal:'',
          proDetailInfos: [],
          cdProInfos: {},//传递给子组件的数据
          getPrice: '',
        }
      },
      created(){
        store.commit('footShow');
        //页面初始化的时候获取全部的挂单商品
        this.$http({
          method: 'get',
          url: this.url + '/api/sub/getHangGoods',
          header: {
            'Content-type': 'application/x-www-form-urlencoded'
          },
        }).then(res => {

          if (res.data.error_code === 0){

            this.proDetailInfos = res.data.data.list;
            console.log(this.proDetailInfos)
          }
        }).catch(error => {
          ;
        });

        this.$http({
          method: 'get',
          url: this.url + '/api/sub/getSubGoods',
          header: {
            'Content-type': 'application/x-www-form-urlencoded'
          },
          params: {
            status: 2,
          }
        }).then(res => {
          if (res.data.error_code === 0){
            this.gxProInfos = res.data.data.list;
          }
        }).catch(error => {
          ;
        });
        // Toast('请先选择左上角的产品');
      },
      methods: {
        changePro(){
          console.log(this.morenVal);
          //只有在选择了产品后才能发请求
          this.$http({
            method:'get',
            url: this.url + '/api/sub/getHangGoods',
            header: {
              'Content-type': 'application/x-www-form-urlencoded'
            },
            params: {
              'goodsId': this.morenVal,
            }
          }).then(res => {
            if (res.data.error_code === 0){
              this.proDetailInfos = res.data.data.list;
            }
          }).catch(error => {
            ;
          });

          //发起请求获取商品当前单价
          this.$http({
            method:'get',
            url: this.url + '/api/sub/getSubGoodsById',
            header: {
              'Content-type': 'application/x-www-form-urlencoded'
            },
            params: {
              'id': this.morenVal,
            }
          }).then(res => {
            if (res.data.error_code === 0){
              this.getPrice = res.data.data.goodsInfo.goodsCost;
              this.imgUrls = res.data.data.goodsInfo.imgLoadPath
            }
          }).catch(error => {

          });
        },
        checkCur(ziIndex,ziItem){
          this.active = this.active==ziIndex? -1 : ziIndex;
          if(this.active == ziIndex){
            this.initState = true;
          }else {
            this.initState = false;
          }
          this.cdProInfos.proid = ziItem.goodsId;//商品id
          this.cdProInfos.gdproid = ziItem.id;//挂单商品id
          this.cdProInfos.num = ziItem.holdNum;
          this.cdProInfos.dPrice = this.getPrice;
          this.cdProInfos.proname = ziItem.goodsName;
          this.cdProInfos.gdmobile = ziItem.mobile;
          this.cdProInfos.proCurPrice = this.getPrice;
        }
      },
      components: {pro_list}
    }
</script>

<style scoped>
  .mailheader {
    background: #2eae64;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: .2rem ;
  }
  .proTypeName {
    background: #178f4a;
    width: 40%;
    overflow: hidden;
    height: .7rem;
    padding: .1rem;
    color: #fff;
  }
  .pro_list_content {
    width: 100%;
    position: relative;
  }
  .pro_list_content li {
    width: 49%;
    background: #ffffff;
    margin-bottom: .2rem;
  }
  .pro_list_content li div.pro_img_content {
    width: 96%;
    margin: auto;
  }
  .pro_list_content {
    width: 100%;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    align-items: center;
  }
  .pro_detail {
    padding: .2rem .4rem;
    line-height: 2;
    text-align: left;
    position: relative;
  }
  #sel_circle {
    position: absolute;
    width: .6rem;
    height: .6rem;
    bottom: .4rem;
    right: .3rem;
    background: url("./mallImgs/product_nocheck.png") center center no-repeat;
    background-size: contain;
  }
  #sel_circle.active {
    background: url("./mallImgs/product_checked.png") center center no-repeat;
    background-size: contain;
  }

</style>
