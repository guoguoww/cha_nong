<template>
  <div class="storeFooterContent">
    <ul class="storeFooterW">
      <li>
        <img src="./meImgs/house_have.png" alt="">
        <span>持仓</span>
      </li>
      <li>
        <img src="./meImgs/house_change.png" alt="">
        <span>转让</span>
      </li>
      <li>
        <img src="./meImgs/house_take.png" alt="">
        <span>提货</span>
      </li>
    </ul>
  </div>
</template>

<script>
    export default {
        name: "store-man-com-footer"
    }
</script>

<style scoped>
  .storeFooterContent {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
  }
  .storeFooterW {
    background: #fff;
    padding: .2rem 0;
    display: flex;
    justify-content: space-around;
    align-items: center;
    border-top: 1px solid #ddd;
  }
  .storeFooterW li {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .storeFooterW li img {
    width: .4rem;
    margin-bottom: .1rem;
  }
</style>
