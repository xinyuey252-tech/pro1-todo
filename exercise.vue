<template>
	<view class="container">
		<!-- 运动类型选择 -->
		<scroll-view scroll-x class="sport-scroll">
			<view class="sport-tag" 
			      v-for="(item, index) in sportTypes" 
			      :key="index"
			      :class="{ active: currentSportIndex === index }"
			      @tap="onSwiperChange({ detail: { current: index } })">
				<image :src="item.icon" class="sport-icon-mini" mode="aspectFit" />
				<text>{{ item.name }}</text>
			</view>
		</scroll-view>

		<!-- 运动数据展示 -->
		<view class="sport-data-grid">
			<view class="data-card">
				<image src="/static/time.png" class="card-icon" />
				<text class="label">持续时间</text>
				<text class="value">{{ formatDuration }}<text class="unit">分钟</text></text>
			</view>
			<view class="data-card">
				<image src="/static/fire.png" class="card-icon" />
				<text class="label">消耗卡路里</text>
				<text class="value">{{ calories }}<text class="unit">千卡</text></text>
			</view>
			<view class="data-card">
				<image src="/static/heart.png" class="card-icon" />
				<text class="label">平均心率</text>
				<text class="value">{{ heartRate }}<text class="unit">bpm</text></text>
			</view>
		</view>

		<!-- 图表展示（点击跳转） -->
		<view class="chart-container" @tap="goToHistory">
			<text class="chart-title">周运动趋势图</text>
			<view class="chart-placeholder" v-if="historyData.week <= 0">
				<image src="/static/chart-empty.png" class="chart-icon" />
				<text class="chart-tip">暂无历史数据，开始运动后即可看到趋势图</text>
			</view>
		</view>

		<!-- 控制按钮 -->
		<view class="controls">
			<view v-if="!isRunning && !isPaused">
				<button type="primary" class="main-button" @tap="startSport">
					<image src="/static/start.png" class="btn-icon" />开始运动
				</button>
			</view>
			<view class="control-row" v-if="isRunning || isPaused">
				<button type="default" v-if="isRunning" @tap="pauseSport">
					<image src="/static/pause.png" class="btn-icon" />暂停
				</button>
				<button type="primary" v-if="isPaused" @tap="continueSport">
					<image src="/static/play.png" class="btn-icon" />继续
				</button>
				<button type="success" @tap="saveRecord">
					<image src="/static/save.png" class="btn-icon" />保存记录
				</button>
			</view>
		</view>

		<!-- 历史记录 -->
		<view class="history-summary">
			<view class="summary-header">
				<text class="summary-title">运动摘要</text>
				<image src="/static/history.png" class="summary-icon" />
			</view>
			<view class="summary-content">
				<view class="summary-item">
					<text class="item-label">本周累计时长:</text>
					<text class="item-value">{{ historyData.week }} 分钟</text>
				</view>
				<view class="summary-item">
					<text class="item-label">本月累计消耗:</text>
					<text class="item-value">{{ historyData.month }} 千卡</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
  data() {
    return {
      sportTypes: [
        { name: "跑步", calorieRate: 10, icon: "/static/running.png" },
        { name: "骑行", calorieRate: 8, icon: "/static/cycling.png" },
        { name: "游泳", calorieRate: 12, icon: "/static/swimming.png" },
        { name: "跳绳", calorieRate: 15, icon: "/static/skipping.png" },
        { name: "快走", calorieRate: 6, icon: "/static/walk-fast.png" },
        { name: "瑜伽", calorieRate: 5, icon: "/static/yoga.png" },
        { name: "力量训练", calorieRate: 7, icon: "/static/Strength.png" }
      ],
      selectedSport: null,
      currentSportIndex: 0,
      duration: 0,
      calories: 0,
      heartRate: 75,
      timer: null,
      startTime: null,
      pausedTime: 0,
      historyData: {
        week: 0,
        month: 0
      },
      isRunning: false,
      isPaused: false
    };
  },
  computed: {
    formatDuration() {
      const minutes = Math.floor(this.duration / 60);
      const seconds = this.duration % 60;
      return `${minutes}:${seconds < 10 ? '0' + seconds : seconds}`;
    }
  },
  mounted() {
    this.loadHistoryData();
    if (this.sportTypes && this.sportTypes.length > 0) {
      this.selectedSport = this.sportTypes[0];
      this.currentSportIndex = 0;
    } else {
      console.warn('运动类型列表为空');
    }
  },
  methods: {
    loadHistoryData() {
      try {
        const historyData = uni.getStorageSync('sportHistoryData');
        if (historyData) {
          this.historyData.week = historyData.week || 0;
          this.historyData.month = historyData.month || 0;
          return;
        }
      } catch (e) {
        console.error('读取缓存失败', e);
      }
      // 默认值
      this.historyData.week = 195;  // 本周累计时长：195分钟
      this.historyData.month = 2369; // 本月累计消耗：2369千卡
    },
    onSwiperChange(e) {
      const newIndex = e.detail.current;
      if (this.sportTypes && this.sportTypes[newIndex]) {
        this.selectedSport = this.sportTypes[newIndex];
        this.currentSportIndex = newIndex;

        // 如果正在运动中，更新卡路里
        if (this.isRunning || this.isPaused) {
          this.updateCalories();
        }
      }
    },
    startSport() {
      if (this.isRunning) return;
      this.startTime = Date.now();
      this.duration = 0;
      this.calories = 0;
      this.pausedTime = 0;
      this.isRunning = true;
      this.isPaused = false;
      this.startTimer();
    },
    startTimer() {
      this.clearTimer();
      this.timer = setInterval(() => {
        const now = Date.now();
        const elapsedSeconds = Math.floor((now - this.startTime) / 1000);
        const totalDuration = this.pausedTime + elapsedSeconds;
        this.duration = totalDuration;
        this.updateCalories();
      }, 1000);
    },
    clearTimer() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = null;
      }
    },
    pauseSport() {
      if (!this.isRunning) return;
      this.clearTimer();
      this.isRunning = false;
      this.isPaused = true;
      this.pausedTime = this.duration;
    },
    continueSport() {
      if (!this.isPaused) return;
      this.startTime = Date.now();
      this.isRunning = true;
      this.isPaused = false;
      this.startTimer();
    },
    saveRecord() {
      const record = {
        sport: this.selectedSport.name,
        duration: Math.floor(this.duration / 60),
        calories: this.calories,
        heartRate: this.heartRate,
        date: new Date().toLocaleString()
      };

      const week = this.historyData.week + Math.floor(this.duration / 60);
      const month = this.historyData.month + this.calories;
      this.historyData.week = week;
      this.historyData.month = month;

      // 保存到缓存
      try {
        uni.setStorageSync('sportHistoryData', {
          week: week,
          month: month
        });
      } catch (e) {
        console.error('保存缓存失败', e);
      }

      // 清除计时器并重置
      this.clearTimer();
      this.isRunning = false;
      this.isPaused = false;
      this.duration = 0;
      this.calories = 0;
      this.pausedTime = 0;

      uni.showToast({
        title: '记录已保存',
        icon: 'success',
        duration: 2000
      });
    },
    updateCalories() {
      if (!this.selectedSport) {
        console.warn('未选择运动类型');
        return;
      }
      const minutes = this.duration / 60;
      const calories = Math.round(minutes * this.selectedSport.calorieRate);
      this.calories = calories;
    },
    // 新增跳转方法
    goToHistory() {
      uni.navigateTo({
        url: '/pages/history/history'
      });
    }
  }
};
</script>

<style scoped>
/* 基础布局 */
.container {
  padding: 20rpx;
  background-color: #f5f7fa;
}

/* 横向运动类型标签 */
.sport-scroll {
  display: flex;
  white-space: nowrap;
  margin-bottom: 40rpx;
  padding-bottom: 10rpx;
}

.sport-tag {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  padding: 20rpx;
  margin-right: 20rpx;
  border-radius: 20rpx;
  background-color: #ffffff;
  color: #666;
  font-size: 24rpx;
  transition: all 0.2s;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.05);
}

.sport-tag.active {
  background-color: #388E3C;
  color: white;
}

.sport-icon-mini {
  width: 60rpx;
  height: 60rpx;
  margin-bottom: 10rpx;
}

/* 网格数据卡片 */
.sport-data-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20rpx;
  margin-bottom: 60rpx;
}

.data-card {
  background: #fff;
  border-radius: 20rpx;
  padding: 20rpx;
  text-align: center;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.04);
}

.card-icon {
  width: 48rpx;
  height: 48rpx;
  margin-bottom: 10rpx;
}

.label {
  font-size: 22rpx;
  color: #888;
}

.value {
  font-size: 36rpx;
  font-weight: bold;
  color: #333;
}

.unit {
  font-size: 22rpx;
  color: #aaa;
  margin-left: 6rpx;
}

/* 控制按钮 */
.controls {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: #fff;
  padding: 20rpx;
  box-shadow: 0 -2rpx 6rpx rgba(0, 0, 0, 0.05);
  z-index: 100;
}

.main-button {
  width: 100%;
  font-size: 32rpx;
  border-radius: 60rpx;
  padding: 24rpx;
}

.control-row {
  display: flex;
  gap: 20rpx;
}

.control-row button {
  flex: 1;
  font-size: 28rpx;
  border-radius: 50rpx;
}

.btn-icon {
  width: 36rpx;
  height: 36rpx;
  margin-right: 12rpx;
}

/* 图表区域 */
.chart-container {
  margin-bottom: 60rpx;
  cursor: pointer; /* 添加可点击指针样式 */
}

.chart-title {
  font-size: 30rpx;
  color: #333;
  margin-bottom: 20rpx;
}

.chart-placeholder {
  background-color: #f0f0f0;
  border-radius: 16rpx;
  padding: 60rpx 30rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.chart-icon {
  width: 100rpx;
  height: 100rpx;
  opacity: 0.4;
  margin-bottom: 20rpx;
}

.chart-tip {
  font-size: 24rpx;
  color: #999;
}

/* 运动摘要 */
.history-summary {
  background-color: #fff;
  padding: 30rpx;
  border-radius: 20rpx;
  margin-bottom: 100rpx;
  box-shadow: 0 4rpx 10rpx rgba(0, 0, 0, 0.05);
}

.summary-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20rpx;
}

.summary-title {
  font-size: 30rpx;
  font-weight: bold;
  color: #333;
}

.summary-icon {
  width: 36rpx;
  height: 36rpx;
}

.summary-content {
  display: flex;
  flex-direction: column;
  gap: 15rpx;
}

.summary-item {
  display: flex;
  justify-content: space-between;
  font-size: 26rpx;
  border-bottom: 1rpx solid #eee;
  padding-bottom: 10rpx;
}

.item-label {
  color: #777;
}

.item-value {
  color: #333;
  font-weight: bold;
}

</style>
