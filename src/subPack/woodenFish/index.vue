<template>
    <image class="image-bg" src="/static/back.png" />
    
    <navBar>
        <template #content>
            <view class="nav">
                <view class="back-icon">
                    <uni-icons type="back" size="26" color="#ffffff" @click="onBack"></uni-icons>
                </view>
                <view class="nav-title">电子木鱼</view>
            </view>
        </template>
    </navBar>
    
    <view class="container" :style="{ paddingTop: globalData.navHeight + 'px' }">
        <view class="content-muyu">
            <view class="list">
                <view v-for="(item, index) of virtuesList" :key="index" class="list-item">
                    {{ item }}
                </view>
            </view>
            <view class="woodfish-box">
                <view class="woodfish" @click="handleKnock">
                    <view class="woodfish-imgbox" :class="{ 'fat-woodfish': isAnimate }">
                        <image src="/static/my.gif" mode=""></image>
                    </view>
                </view>
            </view>
            
            <!-- 功德计数放在木鱼下方 -->
            <view class="accumulate">本次功德：{{ merits }} </view>
        </view>
    </view>
    
    <!-- 底部返回按钮 -->
    <view class="back-button" @click="onBack">
      <button class="back-btn">返回</button>
    </view>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import navBar from '@/components/navBar.vue';
import { getdeviceInfo } from '@/utils/deviceInfo';
const { globalData } = getdeviceInfo();
const audioCtx = uni.createInnerAudioContext();
audioCtx.src = '/static/knock.mp3';
const virtuesList: any = ref([]);
const merits: any = ref(uni.getStorageSync('num') || 0);

// 敲木鱼动画
const isAnimate: any = ref(false);
const animateKnock = () => {
    virtuesList.value.push('功德+1');
    merits.value++;
    uni.setStorageSync('num', merits.value);
    isAnimate.value = !isAnimate.value;
    setTimeout(() => {
        isAnimate.value = !isAnimate.value;
    }, 80);
};
const de_animateKnock = debounce(animateKnock, 80);

function debounce(fn: Function, delay: number) {
    let timer: any = null;
    return function _debounce(...args: any) {
        if (timer) clearTimeout(timer);
        timer = setTimeout(() => {
            fn(...args);
        }, delay);
    };
}

const handleKnock = async () => {
    de_animateKnock();
    audioCtx.stop();
    audioCtx.play();
};

function onBack() {
    uni.navigateBack();
}
</script>

<style scoped lang="scss">
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
    font-size: 30rpx;
    color: #ffffff;
}

.container {
    min-height: 100vh;
    background-size: 100% 100%;
    background-position: 100% 100%;
    .content-muyu {
        position: relative;
        left: 50%;
        top: 25vh !important;
        transform: translateX(-50%);
        .accumulate {
            display: flex;
            justify-content: center;
            font-weight: bolder;
            font-size: 32rpx;
            margin-top: 20rpx; /* 调整与木鱼的间距 */
            color: #fff;
        }
    }
}

.list {
    text-align: center;
    color: #fff;
    display: flex;
    justify-content: center;
}

@keyframes text-animation {
    from {
        top: 0;
        font-size: 32rpx;
        font-weight: bolder;
    }
    to {
        top: -300rpx;
        opacity: 0;
    }
}

.list-item {
    font-size: 28rpx;
    position: absolute;
    animation: text-animation linear 1.5s forwards;
}

@keyframes fat {
    0%, 50% {
        transform: scale(1.1);
    }
}

.fat-woodfish {
    animation: fat;
    animation-duration: 80ms;
}

.woodfish-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .woodfish {
        display: flex;
        justify-content: center;
        align-items: center;
        color: #fff;
        font-size: 100rpx; /* 使用rpx单位 */
        width: 400rpx; /* 使用rpx单位 */
        height: 400rpx; /* 使用rpx单位 */
        margin-top: 50rpx;
        margin-bottom: 50rpx;
        .woodfish-imgbox {
            image {
                width: 180px;
                height: 180px;
                color: #ffffff;
            }
        }
    }

    .title {
        font-size: 40rpx;
        color: #a6a6a6;
    }
}

/* 底部返回按钮样式 */
.back-button {
  display: flex;
  justify-content: center;
  margin-top: 20rpx;
}
</style>
