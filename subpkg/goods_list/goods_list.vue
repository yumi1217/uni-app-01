<template>
  <view>
    <view class="goodsList">
      <view v-for="(item,index) in goodsList" :key="index" @click="gotoDetail(item)">
        <my-goods :item="item"></my-goods>
      </view>
    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        // 请求参数对象
        queryObj: {
          // 查询关键字
          query: '',
          // 分类ID
          cid: '',
          // 页面索引
          pagenum: 1,
          // 每一页长度
          pagesize: 10
        },
        // 商品列表的数据
        goodsList: [],
        // 总数量，用来实现分页
        total: 0,
        // 节流阀：是否正在请求数据
        isLoading: false
      }
    },
    onLoad(options) {
      // console.log(options)
      this.queryObj.query = options.query || ''
      this.queryObj.cid = options.cid || ''
      // 调用获取商品列表数据的方法
      this.getGoodsList()
    },
    methods: {
      // 获取商品列表
      async getGoodsList(cb) {
        // 打开节流阀
        this.isLoading = true
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/goods/search', this.queryObj)
        // console.log(res)
        this.isLoading = false
        cb && cb()
        if (res.meta.status !== 200) return uni.$showMsg('获取商品信息失败')
        this.goodsList = [...this.goodsList, ...res.message.goods]
        this.total = res.message.total
      },
      gotoDetail(item) {
        uni.navigateTo({
          url: '/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id
        })
        // console.log('点击了')
      }
    },
    onReachBottom() {
      if (this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return uni.$showMsg('没有更多数据了')
      if (this.isLoading) return
      // 让页码值自增+1
      this.queryObj.pagenum += 1
      this.getGoodsList()
    },
    onPullDownRefresh() {
      // 重置关键数据
      this.queryObj.pagenum = 1
      this.total = 0
      this.isLoading = false
      this.goodsList = []
      // 重新发起数据请求
      this.getGoodsList(() => uni.stopPullDownRefresh())
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
