<template>
  <view class="container">
    <view
      class="item-btn"
      v-for="(item, index) in btnList"
      :key="index"
      @click="goPage(item.url)"
    >
      <view>
        <view class="title">{{ item.label }}</view>
        <view class="description">{{ item.des }}</view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue';

type list = {
  bgimg: string;
  label: string;
  url: string;
  icon: string;
  des: string;
};

const btnList = ref<list[]>([
  {
    label: '每日一签',
    url: '../../subPack/draw/index',
    des: '心诚则灵 情真则明',
  },
  {
    label: '每日一卦',
    url: '../../subPack/divine/index',
    des: '周易爻卦 预知未来',
  },
  {
    label: '指纹算命',
    url: '../../subPack/fingerprint/index',
    des: '知晓命运 解读自己',
  },
  {
    label: '功德木鱼',
    url: '../../subPack/woodenFish/index',
    des: '积善成德 神明自得',
  },
]);

function goPage(url: string) {
  uni.navigateTo({ url });
}
</script>

<style scoped lang="scss">
.container {
  padding: 30rpx;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly; /* 使项目垂直均匀分布 */
  align-items: center; /* 水平居中对齐 */
  height: 100vh; /* 设定为视口高度，确保项目占满整个屏幕 */
  background: red; /* 设置背景为红色 */
}

.item-btn {
  min-width: 500rpx;
  background-color: yellow; /* 设置按钮背景颜色为黄色 */
  padding: 20rpx 80rpx;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-radius: 120rpx;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
}

.btn-icon {
  height: 188rpx;
  width: 200rpx;
}

.title:nth-child(1) {
  font-size: 32rpx;
  font-weight: bolder;
  color: #8e3a22;
  margin-bottom: 10rpx;
}

.title:nth-child(2) {
  font-size: 24rpx;
  font-weight: bolder;
  color: #8e3a22;
}
</style>
