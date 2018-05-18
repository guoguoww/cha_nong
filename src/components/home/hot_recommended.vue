<template>
    <div class="hot_pro_wrapC">
      <p class="hot_rec_wrap"><span>热门推荐</span><span>更多</span></p>
      <!--<ul >-->
        <!--<li class="hot_list_wrap">-->
          <!--<p>-->
            <!--<img src="./homeImgs/fozhu.png" alt="">-->
          <!--</p>-->
          <!--<div>-->
            <!--<p class="hot_pro_tit_name"><span>天然香绿檀手串顺纹木质佛珠</span></p>-->
            <!--<p><span>商品代码：</span><span>x3753198</span></p>-->
            <!--<p><span>最新价：</span><span>21456</span></p>-->
            <!--<p><span>卖出价：</span><span>21000</span></p>-->
          <!--</div>-->
        <!--</li>-->
        <!--<li class="hot_list_wrap">-->
          <!--<p>-->
            <!--<img src="./homeImgs/fozhu.png" alt="">-->
          <!--</p>-->
          <!--<div>-->
            <!--<p class="hot_pro_tit_name"><span>天然香绿檀手串顺纹木质佛珠</span></p>-->
            <!--<p><span>商品代码：</span><span>x3753198</span></p>-->
            <!--<p><span>最新价：</span><span>21456</span></p>-->
            <!--<p><span>卖出价：</span><span>21000</span></p>-->
          <!--</div>-->
        <!--</li>-->
      <!--</ul>-->
    </div>
</template>

<script>
    export default {
        name: "hot_recommended"
    }
</script>

<style scoped>
  .hot_rec_wrap {
    margin-top: 2px;
    color: #e07701;
    padding: .2rem .4rem;
    background: #ffffff;
    font-weight: bold;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  li.hot_list_wrap  {
    margin-top: 2px;
    padding: .2rem .4rem;
    background: #ffffff;
    display: flex;
    justify-content: flex-start;
    border-bottom: 1px solid #f5f5f5;
  }
  li.hot_list_wrap > p {
    width: 30%;
  }
  li.hot_list_wrap > div {
    width: 70%;
    text-align: left;
    padding-left: .3rem;
    line-height: 1.8;
  }
  li.hot_list_wrap > div > p.hot_pro_tit_name {
    font-weight: bold;
    font-size: .42rem;
  }
</style>
