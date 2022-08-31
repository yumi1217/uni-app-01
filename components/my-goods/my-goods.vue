<template>
  <view class="goodsItem">
    <!-- 左侧的盒子 -->
    <view class="goodsListLeft">
      <radio @click="radioClickHandler" :checked="item.goods_state" color="#C00000" v-if="showRadio"></radio>
      <image class="goodsPic" :src="item.goods_small_logo || defaultPic"></image>
    </view>
    <!-- 右侧的盒子 -->
    <view class="goodsListRight">
      <!-- 商品名字 -->
      <view class="goodsName">{{ item.goods_name }}</view>
      <view class="goodsInfoBox">
        <!-- 商品价格 -->
        <view class="goodsPrice">￥{{ item.goods_price | tofixed }}</view>
        <!-- 商品数量 -->
        <uni-number-box @change="numChangeHandler" :min="1" :value="item.goods_count" v-if="showNum"></uni-number-box>
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
    },
    // 是否展示图片左侧的radio
    showRadio: {
      type: Boolean,
      // 如果外界没有指定show-radio属性的值，则默认不展示radio组件
      default: false
    },
    // 是否展示价格右侧的 NumberBox 组件
    showNum: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      // 默认的空图片
      defaultPic: 'https://img3.doubanio.com/f/movie/8dd0c794499fe925ae2ae89ee30cd225750457b4/pics/movie/celebrity-default-medium.png'
    };
  },
  filters: {
    tofixed(num) {
      return Number(num).toFixed(2);
    }
  },
  methods: {
    // 这是radio组件的点击事件处理函数
    radioClickHandler() {
      this.$emit('radio-change', {
        // 商品的 Id
        goods_id: this.item.goods_id,
        // 商品最新的勾选状态
        goods_state: !this.item.goods_state
      });
    },
    numChangeHandler(val) {
      // console.log(val)
      this.$emit('num-change', {
        goods_id: this.item.goods_id,
        goods_count: +val
      });
    }
  }
};
</script>

<style lang="scss">
.goodsItem {
  // 让 goods-item 项占满整个屏幕的宽度
  width: 750rpx;
  // 设置盒模型为 border-box
  box-sizing: border-box;
  display: flex;
  padding: 10px 5px;
  border-bottom: 1px solid #f0f0f0;

  .goodsListLeft {
    margin-right: 5px;
    display: flex;
    justify-content: space-between;
    align-items: center;

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
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .goodsInfoBox {
      .goodsPrice {
        color: #c00000;
        font-size: 16px;
      }
    }
  }
}
</style>
