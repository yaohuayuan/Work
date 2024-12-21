<template>
	<image class="image-bg" src="../../static/back.png"/>

  <navBar>
    <template #content>
      <view class="nav">
		  
        <view class="back-icon">
          <uni-icons type="back" size="26" color="#ffffff" @click="onBack"></uni-icons>
        </view>
        <view class="nav-title">每日一卦</view>
      </view>
    </template>
  </navBar>

  <!-- 修改此处: 使用动态样式绑定 -->
  <view class="container" :style="{ paddingTop: globalData.navHeight + 'px' }">
    <!-- 卦提示图片 -->
    <!-- <image src="https://aliao-img.oss-cn-hangzhou.aliyuncs.com/feng/day_gua.png" class="hdx-image gua-tip" /> -->

    <view class="gk-box">
      <!-- 卦象图片和硬币动画 -->
      <image :class="[isShake ? 'gk-img gk-img-animate' : 'gk-img']" src="../../static/8g.jpg" />
      
      <view class="coin-list">
        <image v-if="isCoin" :class="['coin-img', 'qianbi-one']" src="../../static/yb1.png" />
        <image v-if="isCoin" :class="['coin-img', 'qianbi-two']" src="../../static/yb2.png" />
        <image v-if="isCoin" :class="['coin-img', 'qianbi-three']" src="../../static/yb3.png" />
      </view>
    </view>

    <!-- 卦象结果展示 -->
    <view class="gua-result" v-if="guaData && isCoin">
      <view>卦名：{{ guaData.book_name }}</view>
      <view>卦辞：{{ guaData.book_means }}</view>
      <view>推断：{{ guaData.book_result }}</view>
      <view>大象：{{ guaData.book_all }}</view>

      <view style="margin-top: 20rpx">简介：{{ guaData.book_introduc }}</view>
      <view>介绍：{{ guaData.book_remark }}</view>

      <view style="margin-top: 20rpx">运势：{{ guaData.book_lucky }}</view>
      <view>疾病：{{ guaData.book_disease }}</view>
      <view>失物：{{ guaData.book_lose }}</view>
      <view>爱情：{{ guaData.book_love }}</view>
      <view>诉讼：{{ guaData.book_action }}</view>
    </view>

    <!-- 抖动按钮 -->
    <view style="display: flex; justify-content: center; margin-top: 40rpx">
      <image src="../../static/ksbg.png" class="btn-img" @click="handleShake" />
    </view>

    <!-- 添加返回按钮 -->
    <view class="back-button">
      <button @click="onBack">返回</button>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import navBar from '@/components/navBar.vue';
import { getdeviceInfo } from '@/utils/deviceInfo';
import { getList } from './divine';

const { globalData } = getdeviceInfo();
const guaData = ref<any>(''); // 存储卦象数据
const isCoin = ref<boolean>(true); // 控制硬币是否显示
const isShake = ref<boolean>(false); // 控制是否抖动

// 返回上一页
function onBack() {
  uni.navigateBack();
}

// 处理抖动事件
function handleShake() {
  isShake.value = true;
  isCoin.value = false; // 隐藏硬币图标
  setTimeout(() => {
    isShake.value = false;
    isCoin.value = true; // 显示硬币图标
    getRandomElementFromArray(); // 获取随机卦象
  }, 2500);
}

// 获取随机卦象
function getRandomElementFromArray() {
  guaData.value = '';
  const arr = getList();
  const randomIndex = Math.floor(Math.random() * arr.length);
  guaData.value = arr[randomIndex];
}
</script>

<style scoped lang="scss">
.nav {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav-title {
  width: 100%;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: 30px;
  color: #ffffff;
}


.container {
  min-height: 100vh;
 
}
.image-bg {
    position: absolute;
    z-index: -1;
    left: 0;
    right: 0;
    top: 0;  /* 确保图片从顶部开始 */
    bottom: 0;  /* 直到页面底部 */
    width: 120%;
    height: 120%;
    object-fit: cover;  /* 保证图片完全覆盖背景区域，保持比例 */
	background-color: red;
}

.hdx-image {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
}

.gua-tip {
  transform: scale(0.8);
  left: 0%;
  width: 100%;
  height: 300rpx;
}

.gk-box {
  color: #fce9b4;
  border-radius: 10rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.gk-img {
  width: 265rpx;
  height: 336rpx;
}

.gk-img-animate {
  animation: shakeAnimate 2.5s 2;
}

@keyframes shakeAnimate {
  0%, to {
    transform: translate(0);
  }
  10%, 20%, 30%, 40% {
    transform: translate(-0.3125rem);
  }
  50%, 60%, 70%, 80%, 90% {
    transform: translate(-0.625rem);
  }
  15%, 25%, 35%, 45% {
    transform: translate(0.3125rem);
  }
  55%, 65%, 75%, 85%, 95% {
    transform: translate(0.625rem);
  }
}

.coin-list {
  display: flex;
  justify-content: space-between;
  width: 12.5rem;
  margin: 0.9375rem 3.75rem;
}

.coin-img {
  width: 2.8875rem;
  height: 2.8875rem;
  vertical-align: top;
}

.qianbi-one, .qianbi-two, .qianbi-three {
  animation: coinAnimation 1.5s 1;
  z-index: 10;
}

@keyframes coinAnimation {
  0% {
    transform: translate(4.375rem, -4.375rem);
  }
  to {
    transform: translate(0);
  }
}

.btn-img {
  height: 114rpx;
  width: 70%;
  animation: btnAnim 1.5s linear infinite;
}

@keyframes btnAnim {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

.gua-result {
  background-size: 100% 100%;
  background-position: 100% 100%;
  padding: 24rpx 70rpx;
  font-size: 28rpx;
  font-weight: bold;
  color: #f3f3f3;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.back-button {
  display: flex;
  justify-content: center;
  margin-top: 30rpx;
}

button {
  background-color: #4CAF50; /* 按钮背景色 */
  color: white; /* 按钮文字颜色 */
  padding: 10px 20px; /* 按钮内边距 */
  font-size: 18px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049; /* 按钮悬停时的背景色 */
}
</style>
