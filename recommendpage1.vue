<!-- <template>
	<view>
		这是健康知识推送页面1。
		<text>推荐详情页面</text>
		<text>当前ID: {{ currentId }}</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				currentId: ''
			}
		},
		onLoad(options) {
			// 获取传递过来的 id 参数
			this.currentId = options.id;
		}
	}
</script>

<style>

</style> -->




<template>
  <view class="container">
    <scroll-view class="content" scroll-y>
      <view class="section">
        <text class="section-title">今日推荐：</text>
        <view class="card">
          <text>• 多喝水，保持身体水分。</text>
        </view>
        <view class="card">
          <text>• 适量运动，每天 30 分钟快走。</text>
        </view>
        <view class="card">
          <text>• 保持充足睡眠，7–8 小时为宜。</text>
        </view>
      </view>
      <view class="section">
        <text class="section-title">健康提示：</text>
        <view class="card">
          <text>• 饭后半小时再运动，避免不适。</text>
        </view>
        <view class="card">
          <text>• 均衡膳食，多吃蔬果。</text>
        </view>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  methods: {
    // goBack() {
    //   uni.navigateBack();
    // }
  }
};
</script>

<style scoped>
.container {
  flex: 1;
  background-color: #F0F4F8;
  padding-top: 20rpx;
}

/* 如果想要保留导航栏色块，可以加上 header */
.header {
  height: 100rpx;
  background-color: #81C784;
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 20rpx; /* 顶部状态栏留白 */
}
.header .title {
  color: #fff;
  font-size: 36rpx;
  font-weight: bold;
}

/* 滚动内容外层 */
.content {
  padding: 20rpx;
}

/* 每个区块 */
.section {
  margin-bottom: 40rpx;
}

/* 区块标题 */
.section-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 20rpx;
}

/* 删除首行小图标伪元素 */
.card text:first-child::before {
  content: none;
}

/* 卡片容器，改为绿色系背景 */
.card {
  background-color: #E8F5E9;    /* 淡绿色背景 */
  padding: 30rpx;
  border-radius: 16rpx;
  margin-bottom: 20rpx;
  /* iOS 阴影 */
  box-shadow: 0 4rpx 8rpx rgba(0, 128, 0, 0.1);
  /* 安卓阴影 */
  elevation: 4;
  line-height: 1.6;
}

/* 卡片中的文字，去掉前面的 • */
.card text {
  font-size: 28rpx;
  color: #2E7D32;   /* 深绿色文字 */
  /* 如果想让行首对齐，可加左内边距 */
  padding-left: 0;
}

/* 也可以给整个 content 区域加一层浅绿色背景框 */
.content {
  padding: 20rpx;
  background-color: #F1F8E9;  /* 更浅的绿色底层 */
  border-radius: 12rpx;
}


/* Scroll-view 自带的滚动条不够显眼，可以加个内边距 */
scroll-view.content {
  padding-bottom: 30rpx;
}

</style>

















