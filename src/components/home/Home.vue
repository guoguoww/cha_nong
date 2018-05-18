<template>
  <div>
    <homeheader></homeheader>
    <noticebar></noticebar>
    <div v-show="defaultShow">
      <div class="ms_content">
        <div class="ms_wrap">
          <div class="ms_img">
            <div>
              <span>买卖街秒杀: </span>
              <span>0</span>
            </div>
            <p><span>零售价: </span><span class="colorR">&yen;0</span></p>
          </div>
          <p class="bgR">
            <a href="javascript:;" @click="buy_sell_pro">
              <img src="./homeImgs/home_onebuy.png" alt="">
              <span class="buy_yj">一键买入</span>
            </a>
            <a href="javascript:;" @click="buy_sell_pro">
              <span class="sel_yj">一键卖出</span>
              <img src="./homeImgs/home_onesell.png" alt="">
            </a>
          </p>
        </div>
        <div class="msc_wrap">
          <!--倒计时组件的位置-->
          <div class="yjms_price">
            <div class="ms_back">
              <div class="ms_price_content">
                <p><span>秒杀基准价: </span><span></span></p>
                <p><span>当前成交价: </span><span></span></p>
              </div>
              <p class="ms_back_timeC">
                <span>秒杀倒计时 </span>
                <span class="bg_black">00</span>:
                <span class="bg_black">00</span>:
                <span class="bg_black">00</span>
              </p>
            </div>
            <div class="back_wrapper">
              <p class="num_price" >
                <span class="bg_f">0</span>
                <span class="bg_f">0</span>
                <span class="bg_f">0</span>
                <span class="bg_f">0</span>
                <span class="bg_f">0</span>
                <span class="bg_f">.</span>
                <span class="bg_f">0</span>
                <span class="bg_f">0</span>
              </p>
              <p class="ch_price">
                <span>万</span>
                <span>千</span>
                <span>百</span>
                <span>十</span>
                <span>个</span>
                <i>·</i>
                <span>角</span>
                <span>分</span>
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <van-swipe @change="onChange" :show-indicators="false"  v-show="xunhunShow">
      <van-swipe-item  v-for = "proItem of gxProListInfos">
        <div class="ms_content">
          <div class="ms_wrap">
            <div class="ms_img">
              <div>
                <span>买卖街秒杀: </span>
                <span>{{proItem.goodsName}}</span>
              </div>
              <p><span>零售价: </span><span class="colorR">&yen;{{parseFloat(curProPrice)}}</span></p>
            </div>
            <p class="bgR">
                <a href="javascript:;" @click="buy_sell_pro">
                  <img src="./homeImgs/home_onebuy.png" alt="">
                  <span class="buy_yj">一键买入</span>
                </a>
              <a href="javascript:;" @click="buy_sell_pro">
                <span class="sel_yj">一键卖出</span>
                <img src="./homeImgs/home_onesell.png" alt="">
              </a>
            </p>
          </div>
          <div class="msc_wrap">
            <!--倒计时组件的位置-->
            <div class="yjms_price">
              <div class="ms_back">
                <div class="ms_price_content">
                  <p><span>秒杀基准价: </span><span></span></p>
                  <p><span>当前成交价: </span><span></span></p>
                </div>
                <p class="ms_back_timeC">
                  <span>秒杀倒计时 </span>
                  <span class="bg_black">{{phours}}</span>:
                  <span class="bg_black">{{pmintues}}</span>:
                  <span class="bg_black">{{psecondes}}</span>
                </p>
              </div>
              <div class="back_wrapper">
                <p class="num_price" >
                  <span class="bg_f" v-for="priceItem of curProPrice">{{priceItem}}</span>
                </p>
                <p class="ch_price">
                  <span>万</span>
                  <span>千</span>
                  <span>百</span>
                  <span>十</span>
                  <span>个</span>
                  <i>·</i>
                  <span>角</span>
                  <span>分</span>
                </p>
              </div>
            </div>
          </div>
        </div>
      </van-swipe-item>
    </van-swipe>
    <!--<div class="kuaixun">-->
      <!--<div>-->
        <!--<img src="./homeImgs/home_news.png" alt="">-->
      <!--</div>-->
      <!--<ul>-->
        <!--<router-link to="/gongGCenter" tag="div">-->
          <!--<li v-for="zixunItem of zixunListInfos">{{zixunItem.title}}</li>-->
        <!--</router-link>-->
      <!--</ul>-->
    <!--</div>-->
    <homelist></homelist>
    <hotrecommend></hotrecommend>
    <div class="home_apply">
      <p><span>合作单位: </span><span>品道收藏</span></p>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue';
  import {Toast,Swipe, SwipeItem} from 'vant';
  Vue.use(Toast);
  Vue.use(Swipe).use(SwipeItem);
  import homelist from './gnlist';
  import noticebar from './noticeBar';
  import homeheader from './Home_header';
  import hotrecommend from './hot_recommended';
  import store from '@/vuex/index';
  export default {
    name: 'Home',
    data () {
      return {
        zixunListInfos: [],
        gxProListInfos: [],
        curProPrice: '',
        curProId: 0,
        phours: '0'+0,
        pmintues: '',
        psecondes: '',
        defaultShow: false,
        xunhunShow: true,
        systermTime: '',
        wan: '',
        qian: '',
        bai: '',
        shi: '',
        ge: '',
        jiao: '',
        fen: '',
      }
    },
    created(){
      store.commit('footShow');
      /*进入页面要先获取当前的系统时间*/

      this.$http({
        method: 'get',
        url: this.url + '/api/sub/getSysTime',
      }).then(res => {
        if (res.data.error_code === 0){
          this.systermTime = res.data.data.sysTime;
          this.pmintues = 59 - parseInt(this.systermTime.split(' ')[1].split(':')[1]);
          this.psecondes = 60 - parseInt(this.systermTime.split(' ')[1].split(':')[2]);
        }else {
          Toast('请求的error_code不为0');
        }
      }).catch(error => {

      });
      /*首页页面倒计时*/
      setInterval(this.pbackTime,1000);
      //获取购销产品id
      this.$http({
        method:'get',
        url: this.url + '/api/sub/getSubGoods',
        header: {
          'Content-type': 'application/x-www-form-urlencoded',
        },
        params: {
          'status':'2'
        }
      }).then(res => {
        if (res.data.error_code === 0){
          if (res.data.data.list!=''){
            this.xunhunShow = true;
            this.defaultShow =false;
            this.gxProListInfos = res.data.data.list;
          }else {
            this.defaultShow =true;
            this.xunhunShow = false;
          }
        }else {
          Toast('请求的error_code不为0');
        }
        this.getGoodsPrice(this.gxProListInfos[0].id);
      }).catch(error => {
      });

      /*获取公告信息*/
      this.$http({
        method: 'get',
        url: this.url + '/api/index/getNewsFlash',
        header: {
          'Content-type': 'application/x-www-form-urlencoded',
        },
        parmas: {
          type: '2'
        }
      }).then(res => {
        if (res.data.error_code === 0){
          this.zixunListInfos = res.data.data.list;
        }
      }).catch(error => {

      })
    },
    methods: {
      getGoodsPrice(id) {
        this.$http({
          method:'get',
          url: this.url + '/api/sub/getSubGoodsById',
          header: {
            'Content-type': 'application/x-www-form-urlencoded',
          },
          params: {
            'id': id
          }
        }).then(res => {
          if (res.data.data.goodsInfo.goodsCost != 0){
            this.curProPrice = parseFloat(res.data.data.goodsInfo.goodsCost).toString();
          }else {
            this.curProPrice = parseFloat(res.data.data.goodsInfo.goodsSubPrice).toString();
          }

          let curPricelength = this.curProPrice.length;
          //先判断当前价格里面有没有小数点
          if (this.curProPrice.indexOf('.')>0){//找不到会返回-1，这个是说明找到了
            //要判断小数点后面的位数
            let xsNumLength = this.curProPrice.split('.')[1].length;
            if (xsNumLength == 2){//说明小数位是2位数了，那么要判断整数位数是否为5位
              if(curPricelength == 8){
                this.curProPrice = this.curProPrice;
              }else if (curPricelength == 7){
                this.curProPrice = '0'+this.curProPrice;
              }else if (curPricelength == 6){
                this.curProPrice = '00'+this.curProPrice;
              }else if (curPricelength == 5){
                this.curProPrice = '000'+this.curProPrice;
              }else if (curPricelength == 4){
                this.curProPrice = '0000'+this.curProPrice;
              }
            }else {//小数点位数为1，后面需要补0，
              if(curPricelength == 7){
                this.curProPrice = this.curProPrice+'0';
              }else if (curPricelength == 6){
                this.curProPrice = '0'+this.curProPrice+'0';
              }else if (curPricelength == 5){
                this.curProPrice = '00'+this.curProPrice+'0';
              }else if (curPricelength == 4){
                this.curProPrice = '000'+this.curProPrice+'0';
              }else if (curPricelength == 3){
                this.curProPrice = '0000'+this.curProPrice+'0';
              }
            }
          }else {//没找到小数点，说明此时的curPricelength位数完全是整数位
            if(curPricelength == 5){
              this.curProPrice = this.curProPrice+'.'+'00';
            }else if (curPricelength == 4){
              this.curProPrice = '0'+this.curProPrice+'.'+'00';
            }else if (curPricelength == 3){
              this.curProPrice = '00'+this.curProPrice+'.'+'00';
            }else if (curPricelength == 2){
              this.curProPrice = '000'+this.curProPrice+'.'+'00';
            }else if (curPricelength == 1){
              this.curProPrice = '0000'+this.curProPrice+'.'+'00';
            }
          }
        }).catch(error => {
          ;
        });
      },
      pbackTime(){
        if (this.pmintues > 0){
          this.psecondes --;
          if (this.psecondes == 0){
            this.pmintues --;
            this.psecondes = 59;
          }
        }else {
          //调取获取当前单价的接口函数
          this.getGoodsPrice(this.gxProListInfos[this.curProId].id);
          this.pmintues = 59;
        }
      },
      onChange(index) {
        this.curProId = index;
        //获取产品当前价格
        this.getGoodsPrice(this.gxProListInfos[index].id);
      },
      buy_sell_pro(){
        Toast('正在开发中...');
      }
    },
    components:{
      homelist,
      homeheader,
      noticebar,
      hotrecommend
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .ms_price_content {
    font-size: .4rem;
  }
  .kuaixun img {
    width: 1.4rem;
    margin-right: .3rem;
    margin-top: .1rem;
    vertical-align: middle;
  }
  .kuaixun > ul {
    width: 80%;
  }
  a {
    color: #42b983;
  }
  .back_wrapper {
    margin-top: .5rem;
  }
  .num_price span {
    display: inline-block;
    padding: .1rem .3rem;
    font-size: .6rem;
    margin-right: .1rem;
  }
  .ch_price span{
    display: inline-block;
    width: 1rem;
    font-size: .4rem;
    margin-top: .2rem;
  }
  .bg_f {
    background: #fff;
    border-radius: .1rem;
  }
  .bg_r {
    background: #fff;
    border-radius: .1rem;
    color: #E21918;
    font-size: .5rem !important;
    width: .7rem;
    padding: 0 .1rem!important;
  }
  .ms_content {
    background: #fff;
    padding: .1rem .3rem;
    font-size: 14px;
    padding-top: .2rem;
  }
  .bg_black {
    background: #000000;
    color: #fff;
    display: inline-block;
    padding: 0 .05rem;
    padding-top: .05rem;
    border-radius: .1rem;
  }
  .colorR {
    color: #E21918;
    font-size: .5rem;
  }
  .bgR a {
    color: #fff;
    margin-right: .1rem;
  }
  .bgR a img {
    width: 2rem;
  }
  .ms_wrap{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: .3rem;
  }
  .msc_wrap {
    width: 98%;
    margin: auto;
    background: #f5f5f5;
  }
  .buy_sel {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: .4rem;
    margin-bottom: .2rem;
    font-size: .3rem;
  }
  .buy_sel img {
    width: 2rem;
    margin-right: .05rem;
  }
  .ms_back_timeC {
    font-size: .4rem;
  }
  .bgR {
    color: #fff;
    position: relative;
    font-size: .3rem;
  }
  .bgR span.buy_yj {
    position: absolute;
    top: .15rem;
    left: .3rem;
  }
  .sel_yj {
    position: absolute;
    top: .15rem;
    right: .4rem;
  }
  .yjms_price {
    background: #f5f5f5;
    padding: .3rem;
    font-size: .3rem;
  }
  .ms_back {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .home_apply {
    width: 100%;
    background: #f5f5f5;
    margin-top: .1rem;
    color: #553006;
    text-align: left;
    padding: .2rem .4rem;
    font-size: .4rem;
    font-weight: bold;
    margin-bottom: 1.5rem;
  }
  .ms_img {
    font-size: .42rem;
    line-height: 1.5;
    text-align: left;
  }
  .ms_img img{
    width: .7rem;
    margin-right: .2rem;
    vertical-align: middle;
  }
  .all_ms_img img {
    width: 1.6rem;
  }

</style>
