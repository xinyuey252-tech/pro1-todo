<template>
  <view class="container">
    <scroll-view class="content" scroll-y>
      <view class="section">
        <text class="section-title">今日推荐：</text>
        <view class="card">
          <text>• 工作间隙做深呼吸，缓解压力。</text>
        </view>
        <view class="card">
          <text>• 保持正确坐姿，避免腰背劳损。</text>
        </view>
        <view class="card">
          <text>• 每天写日记，增强心理健康。</text>
        </view>
      </view>
      <view class="section">
        <text class="section-title">健康提示：</text>
        <view class="card">
          <text>• 午后喝一杯蜂蜜柠檬水，补充维生素。</text>
        </view>
        <view class="card">
          <text>• 睡前泡脚，提高睡眠质量。</text>
        </view>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  name: 'Push3',
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
