<template>
  <view>
    <!-- 用户未登录时，展示登录组件 -->
    <my-login v-if="!token"></my-login>
    <!-- 用户登录以后，展示用户信息组件 -->
    <my-userinfo v-else></my-userinfo>
  </view>
</template>

<script>
import badgeMix from '../../mixins/tabbar-badge.js';
// 1. 从 vuex 中按需导入 mapState 辅助函数
import { mapState } from 'vuex';
export default {
  mixins: [badgeMix],
  computed: {
    ...mapState('m_user', ['token'])
  },
  data() {
    return {};
  }
};
</script>

<style lang="scss">
page,
.my-container {
  height: 100%;
}
</style>
