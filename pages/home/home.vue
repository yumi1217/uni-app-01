<template>
  <view>
    <!-- 使用自定义的搜索组件 -->
    <view class="search-box">
      <my-search @click="gotoSearch"></my-search>
    </view>
    <!-- 轮播图区域 -->
    <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :current="true">
      <swiper-item v-for="(item,index) in swiperList" :key="index">
        <navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
          <image :src="item.image_src"></image>
        </navigator>
      </swiper-item>
    </swiper>
    <!-- 分类导航区域 -->
    <view class="nav-list">
      <view @click="navClickHander(item)" class="nav-item" v-for="(item,index) in navList" :key="index">
        <image class="nav-img" :src="item.image_src"></image>
      </view>
    </view>
    <!-- 楼层区域 -->
    <view class="floor-list">
      <!-- 楼层 item 项 -->
      <view class="floor-item" v-for="(item, index) in floorList" :key="index">
        <!-- 楼层标题 -->
        <image :src="item.floor_title.image_src" class="floor-title"></image>
        <!-- 楼层图片区域 -->
        <view class="floor-img-box">
          <!-- 左侧大图片的盒子 -->
          <navigator class="left-img-box" :url="item.product_list[0].url">
            <image :src="item.product_list[0].image_src" :style="{width: item.product_list[0].image_width + 'rpx'}"
              mode="widthFix"></image>
          </navigator>
          <!-- 右侧 4 个小图片的盒子 -->
          <view class="right-img-box">
            <navigator :url="item2.url" class="right-img-item" v-for="(item2, i2) in item.product_list" :key="i2" v-if="i2 !== 0">
              <image :src="item2.image_src" mode="widthFix" :style="{width: item2.image_width + 'rpx'}"></image>
            </navigator>
          </view>
        </view>
      </view>
    </view>
  </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        // 这是轮播图的数据列表
        swiperList: [],
        // 分类导航的数据
        navList: [],
        // 楼层数据
        floorList: []
      };
    },
    // uniapp里面inLoad相当于vue的created
    onLoad() {
      // 调用方法，获取轮播图数据
      this.getSwiperList()
      // 调用方法，获取nav数据
      this.getNavList()
      // 调用方法，获取楼层数据
      this.getFloor()
    },
    methods: {
      // 获取轮播图的方法
      async getSwiperList() {
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/home/swiperdata')
        // console.log(res)
        // 请求失败
        if (res.meta.status !== 200) return uni.$showMsg()
        this.swiperList = res.message
      },
      // 获取导航栏数据的函数
      async getNavList() {
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/home/catitems')
        // console.log(res)
        // 请求失败
        if (res.meta.status !== 200) return uni.$showMsg()
        this.navList = res.message
      },
      // 点击导航栏的函数
      navClickHander(item) {
        // console.log('点击成功',item)
        if (item.name === '分类') {
          uni.switchTab({
            url: '/pages/cate/cate'
          })
        }
      },
      // 获取首页楼层数据的方法
      async getFloor() {
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/home/floordata')
        // console.log(res)
        // 请求失败
        if (res.meta.status !== 200) return uni.$showMsg()
        // 对数据进行处理
        res.message.forEach(floor => {
          floor.product_list.forEach(prod => {
            prod.url = '/subpkg/goods_list/goods_list?' + prod.navigator_url.split('?')[1]
          })
        })
        this.floorList = res.message
      },
      gotoSearch() {
        uni.navigateTo({
          url: '/subpkg/search/search'
        })
      }
    }
  }
</script>

<style>
  /* 轮播图区域 */
  swiper {
    height: 330rpx;
  }

  swiper image {
    width: 100%;
    height: 100%;
  }

  .swiper-item {
    width: 100%;
    height: 100%;
  }

  /* 轮播图区域 */

  /* 导航栏区域 */
  .nav-list {
    display: flex;
    justify-content: space-around;
    margin: 15px 0;
  }

  .nav-list .nav-img {
    width: 128rpx;
    height: 140rpx;
  }

  /* 导航栏区域 */

  /* 楼层区域 */
  /* 楼层标题 */
  .floor-title {
    width: 100%;
    height: 60rpx;
    display: flex;
  }

  /* 楼层图片区域 */
  .right-img-box {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }

  .floor-img-box {
    display: flex;
    padding-left: 10rpx;
  }

  /* 楼层区域 */
  
  .search-box {
    // 设置定位效果为“吸顶”
    position: sticky;
    // 吸顶的“位置”
    top: 0;
    // 提高层级，防止被轮播图覆盖
    z-index: 999;
  }
</style>
