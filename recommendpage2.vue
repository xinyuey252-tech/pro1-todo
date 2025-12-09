<template>
  <view class="container">
    <scroll-view class="content" scroll-y>
      <view class="section">
        <text class="section-title">今日推荐：</text>
        <view class="card">
          <text>• 每天早晨空腹喝一杯温水，促进新陈代谢。</text>
        </view>
        <view class="card">
          <text>• 午餐后散步10分钟，帮助消化。</text>
        </view>
        <view class="card">
          <text>• 晚餐少吃荤腥，多吃清淡蔬菜。</text>
        </view>
      </view>
      <view class="section">
        <text class="section-title">健康提示：</text>
        <view class="card">
          <text>• 经常眨眼，预防眼睛干涩。</text>
        </view>
        <view class="card">
          <text>• 每小时站起来活动5分钟，防久坐。</text>
        </view>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  name: 'Push2',
  methods: {
    // 自定义逻辑可加在这里
  }
};
</script>

<style scoped>
.container {
  flex: 1;
  background-color: #F0F4F8;
  padding-top: 20rpx;
}
.content {
  padding: 20rpx;
  background-color: #F1F8E9;
  border-radius: 12rpx;
}
.section {
  margin-bottom: 40rpx;
}
.section-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 20rpx;
}
.card {
  background-color: #E8F5E9;
  padding: 30rpx;
  border-radius: 16rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 8rpx rgba(0, 128, 0, 0.1);
  elevation: 4;
  line-height: 1.6;
}
.card text {
  font-size: 28rpx;
  color: #2E7D32;
  padding-left: 0;
}
</style>
