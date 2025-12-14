<template>
  <view class="calendar-container">
    <text class="month-title">六月打卡情况</text>
    <view class="calendar-grid">
      <view
        v-for="day in days"
        :key="day.date"
        class="day-box"
        :class="day.status"
      >
        <text class="day-text">{{ day.label }}</text>
      </view>
    </view>
    <button class="calendar-punch-btn" @tap="handlePunch">打 卡</button>
  </view>
</template>

<script>
export default {
  data() {
    return {
      today: 17,
      days: []
    };
  },
  created() {
    this.generateCalendar();
  },
  methods: {
    generateCalendar() {
      const days = [];
      for (let i = 1; i <= 30; i++) {
        let status = 'none';
        if (i <= 16) status = 'done';
        if (i === this.today) status = 'pending';
        days.push({ label: `6.${i}`, date: i, status });
      }
      this.days = days;
    },
    handlePunch() {
      const index = this.days.findIndex(d => d.date === this.today);
      if (index !== -1) {
        this.days[index].status = 'done';
        uni.showToast({ title: '打卡成功!', icon: 'success' });
      }
    }
  }
};
</script>

<style scoped>
.calendar-container {
  padding: 30rpx;
  background: #f3f4f6;
  border-radius: 16rpx;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  max-width: 700rpx;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.month-title {
  font-size: 36rpx;
  font-weight: bold;
  color: #444;
  text-align: center;
  margin-bottom: 20rpx; /* 调整上下间距 */
  width: 100%;  /* 让标题占满父容器宽度 */
}

.calendar-grid {
  padding: 30rpx 10rpx;
  display: flex;
  flex-wrap: wrap;
  gap: 18rpx;
}

.day-box {
  width: 80rpx;
  height: 80rpx;
  background: #e0e0e0;
  border-radius: 16rpx;
  align-items: center;
  justify-content: center;
  display: flex;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.day-box:hover {
  transform: scale(1.05);
}

.day-box.done {
  background-color: #66bb6a;
  color: #fff;
}

.day-box.pending {
  background-color: #ffb74d;
  color: #fff;
}

.day-box.none {
  background-color: #cfd8dc;
}

.day-text {
  font-size: 24rpx;
  color: #333;
}

.calendar-punch-btn {
  margin-top: 40rpx;
  background: linear-gradient(45deg, #ff7043, #ff5722);
  color: #fff;
  font-size: 28rpx;
  padding: 20rpx;
  border-radius: 50px;
  width: 100%;
  border: none;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.calendar-punch-btn:hover {
  background: linear-gradient(45deg, #ff5722, #ff7043);
  transform: scale(1.05);
}

.calendar-punch-btn:active {
  background: #f4511e;
}
</style>
