<template>
  <view class="goodsItem">
    <!-- 左侧的盒子 -->
    <view class="goodsListLeft">
      <image class="goodsPic" :src="item.goods_small_logo || defaultPic"></image>
    </view>
    <!-- 右侧的盒子 -->
    <view class="goodsListRight">
      <!-- 商品名字 -->
      <view class="goodsName">
        {{ item.goods_name }}
      </view>
      <view class="goodsInfoBox">
        <!-- 商品价格 -->
        <view class="goodsPrice">￥{{item.goods_price | tofixed}}</view>
      </view>
    </view>
  </view>
</template>

<script>
  export default {
    props: {
      item: {
        type: Object,
        default: {}
      }
    },
    data() {
      return {
        // 默认的空图片
        defaultPic: 'https://img3.doubanio.com/f/movie/8dd0c794499fe925ae2ae89ee30cd225750457b4/pics/movie/celebrity-default-medium.png'
      }
    },
    filters:{
      tofixed(num){
        return Number(num).toFixed(2)
      }
    }
  }
</script>

<style lang="scss">
  .goodsItem {
    display: flex;
    padding: 10px 5px;
    border-bottom: 1px solid #f0f0f0;

    .goodsListLeft {
      margin-right: 5px;

      .goodsPic {
        width: 100px;
        height: 100px;
        display: block;
      }
    }

    .goodsListRight {
      display: flex;
      flex-direction: column;
      justify-content: space-between;

      .goodsName {
        font-size: 13px;
      }

      .goodsInfoBox {
        .goodsPrice {
          color: #C00000;
          font-size: 16px;
        }
      }
    }
  }
</style>
