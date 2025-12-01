<template>
  <scroll-view class="container" scroll-y>
    <!-- 顶部用户卡片 -->
    <view class="user-card">
      <view class="avatar-wrap" @tap="chooseAvatar">
        <image :src="user.avatar" class="avatar" mode="aspectFill" />
      </view>
      <view class="user-info-row">
        <view class="user-text">
          <text class="nickname">{{ user.name }}</text>
          <input
            v-if="isEditingSignature"
            class="signature-input"
            v-model="user.signature"
            @blur="toggleEditSignature"
            placeholder="请输入个性签名"
          />
          <text v-else class="signature" @click="toggleEditSignature">
            {{ user.signature }}
          </text>
        </view>
        <button class="edit-btn" @tap="onMenuTap('settings')">编辑个人资料</button>
      </view>
    </view>

    <!-- 每周打卡状态 -->
    <view class="week-punch-status">
      <view
        v-for="(day, index) in weekPunch"
        :key="index"
        class="day"
        :class="day.status"
      >
        <text class="day-label">{{ day.label }}</text>
      </view>
    </view>

    <!-- 打卡按钮 -->
    <view class="punch-btn-wrapper">
      <button
        class="punch-btn"
        :class="{ disabled: hasPunchedToday }"
        @tap="goToPunchPage"
        :disabled="hasPunchedToday"
      >
        {{ hasPunchedToday ? '已打卡' : '去打卡' }}
      </button>
    </view>

    <!-- 数据概览 -->
    <view class="stats">
      <view class="stat-card" v-for="(stat, idx) in stats" :key="idx">
        <text class="stat-value">{{ stat.value }}</text>
        <text class="stat-label">{{ stat.label }}</text>
      </view>
    </view>

    <!-- 我的关注 -->
    <view class="section">
      <text class="section-title">我的关注</text>
      <scroll-view class="follow-list" scroll-x="true" show-scrollbar="false">
        <view class="follow-items-wrapper">
          <view class="follow-item" v-for="f in follows" :key="f.name">
            <image :src="f.avatar" class="follow-avatar" mode="aspectFill" />
            <text class="follow-name">{{ f.name }}</text>
          </view>
        </view>
      </scroll-view>
    </view>

    <!-- 成就展示 -->
    <view class="section">
      <text class="section-title">成就展示</text>
      <view class="badge-list">
        <view
          v-for="b in badges"
          :key="b.id"
          class="badge-item"
        >
          <image :src="b.icon" class="badge-icon" mode="aspectFit" />
          <text class="badge-label">{{ b.name }}</text>
        </view>
      </view>
    </view>

    <!-- 功能入口 -->
    <text class="section-title">功能入口</text>
    <view class="menu">
      <view
        class="menu-item"
        v-for="(item, idx) in menuItems"
        :key="idx"
        @tap="onMenuTap(item.id)"
      >
        <image :src="item.icon" class="menu-icon" mode="aspectFit" />
        <text class="menu-text">{{ item.name }}</text>
      </view>
    </view>
  </scroll-view>
</template>

<script>
export default {
  data() {
    return {
      isEditingSignature: false,
      hasPunchedToday: false,
      user: {
        avatar: '/static/avatar.png',
        name: '张三',
        signature: '今天也要元气满满💪'
      },
      stats: [
        { label: '累计运动(次)', value: 128 },
        { label: '总时长(分钟)', value: 4520 },
        { label: '总消耗(千卡)', value: 38000 }
      ],
      follows: [
        { name: '蒋敦豪', avatar: '/static/id-avatar1.jpg' },
        { name: '鹭卓', avatar: '/static/id-avatar2.jpg' },
        { name: '李耕耘', avatar: '/static/id-avatar3.jpg' },
        { name: '李昊', avatar: '/static/id-avatar4.jpg' },
        { name: '赵一博', avatar: '/static/id-avatar5.jpg' },
        { name: '卓沅', avatar: '/static/id-avatar6.jpg' },
        { name: '赵小童', avatar: '/static/id-avatar7.jpg' },
        { name: '何浩楠', avatar: '/static/id-avatar8.jpg' },
        { name: '陈少熙', avatar: '/static/id-avatar9.jpg' },
        { name: '王一珩', avatar: '/static/id-avatar10.jpg' }
      ],
      badges: [
        { id: 1, icon: '/static/badge1.png' },
        { id: 2, icon: '/static/badge2.png' },
        { id: 3, icon: '/static/badge3.png' },
        { id: 4, icon: '/static/badge4.png' }
      ],
      menuItems: [
        { id: 'records', name: '我的记录', icon: '/static/records.png' },
        { id: 'history', name: '历史趋势', icon: '/static/history.png' },
        { id: 'badges', name: '我的徽章', icon: '/static/badges.png' },
        { id: 'settings', name: '设置', icon: '/static/settings.png' }
      ],
      weekPunch: [
        { label: '周一', status: 'done' },
        { label: '周二', status: 'pending' },
        { label: '周三', status: 'none' },
        { label: '周四', status: 'none' },
        { label: '周五', status: 'none' },
        { label: '周六', status: 'none' },
        { label: '周日', status: 'none' }
      ]
    };
  },
  methods: {
    goToPunchPage() {
      if (this.hasPunchedToday) return;
      this.hasPunchedToday = true;
      this.weekPunch = this.weekPunch.map(day => {
        if (day.label === '周二') {
          return { ...day, status: 'done' };
        }
        return day;
      });
      uni.showToast({ title: '打卡成功!', icon: 'success' });
      setTimeout(() => {
        uni.navigateTo({ url: '/pages/clock/clock' });
      }, 1000);
    },

    onMenuTap(id) {
      const map = {
        records: '/pages/records/records',
        history: '/pages/history/history',  
        badges: '/pages/badges/badges',
        settings: '/pages/setting/setting'
      };
      uni.navigateTo({ url: map[id] });
    },

    chooseAvatar() {
      uni.chooseImage({
        count: 1,
        sizeType: ['original', 'compressed'],
        sourceType: ['album', 'camera'],
        success: (res) => {
          this.user.avatar = res.tempFilePaths[0];
          uni.showToast({
            title: '头像已更新',
            icon: 'success'
          });
        },
        fail: () => {
          uni.showToast({
            title: '选择头像失败',
            icon: 'none'
          });
        }
      });
    },

    toggleEditSignature() {
      this.isEditingSignature = !this.isEditingSignature;
    }
  }
};
</script>

<style scoped>
.container {
  flex: 1;
  background-color: #eaf6f2; /* 非常浅的绿色背景 */
  background: linear-gradient(135deg, #eaf6f2, #c1e9d3); /* 绿色渐变背景 */
}

/* 用户卡片 */
.user-card {
  background: #fff;
  margin: 20rpx;
  padding-top: 90rpx;
  padding-bottom: 20rpx;
  border-radius: 20rpx;
  position: relative;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.1);
}
.avatar-wrap {
  position: absolute;
  top: -10rpx;
  left: 50%;
  transform: translateX(-50%);
  width: 120rpx;
  height: 120rpx;
  border-radius: 60rpx;
  overflow: hidden;
  border: 4rpx solid #81C784;
  background: #fff;
}
.avatar {
  width: 100%;
  height: 100%;
}
.user-info-row {
  margin-top: 70rpx;
  padding: 0 40rpx;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.user-text {
  flex: 1;
  text-align: center;
}
.nickname {
  font-size: 36rpx;
  color: #333;
  font-weight: bold;
}
.signature {
  font-size: 24rpx;
  color: #777;
  margin-top: 20rpx;
}
.signature-input {
  font-size: 24rpx;
  color: #777;
  margin-top: 20rpx;
  width: 100%;
  border: none;
  padding: 8rpx 12rpx;
  border-radius: 10rpx;
  background: #f2f2f2;
}
.edit-btn {
  background-color: #A5D6A7;
  color: #fff;
  font-size: 16rpx;
  padding: 6rpx 12rpx;
  border-radius: 12rpx;
  position: absolute;
  right: 20rpx;
  bottom: 20rpx;
}

/* 今日打卡 */
.punch-btn-wrapper {
  display: flex;
  justify-content: center;
  margin: 20rpx 0;
}

.punch-btn {
  background: #FF7043;
  color: #fff;
  padding: 10rpx 40rpx;
  border-radius: 12rpx;
  font-size: 28rpx;
}
.punch-btn.disabled {
  background: #ccc;
  color: #fff;
}

/* 每周打卡状态 */
.week-punch-status {
  margin: 0 20rpx 20rpx;
  background: #fff;
  padding: 20rpx;
  border-radius: 16rpx;
  display: flex;
  justify-content: space-around;
  box-shadow: 0 2rpx 6rpx rgba(0, 0, 0, 0.05);
}
.day {
  align-items: center;
  display: flex;
  flex-direction: column;
}
.day-label {
  font-size: 24rpx;
  color: #777;
  margin-top: 10rpx;
}
.day.done .day-label {
  color: #4caf50;
  font-weight: bold;
}
.day.pending .day-label {
  color: #ff9800;
  font-weight: bold;
}
.day.none .day-label {
  color: #ccc;
}

/* 数据概览 */
.stats {
  flex-direction: row;
  display: flex;
  justify-content: space-around;
  background-color: #fff;
  margin: 0 20rpx 20rpx;
  padding: 20rpx 0;
  border-radius: 16rpx;
  box-shadow: 0 2rpx 6rpx rgba(0, 0, 0, 0.05);
}
.stat-card {
  align-items: center;
  display: flex;
  flex-direction: column;
}
.stat-value {
  font-size: 32rpx;
  color: #333;
  font-weight: bold;
}
.stat-label {
  font-size: 24rpx;
  color: #777;
  margin-top: 5rpx;
}

/* 我的关注 */
.section {
  margin-top: 30rpx;
}
.section-title {
  font-size: 32rpx;
  font-weight: bold;
  margin-bottom: 20rpx;
}
.follow-list {
  width: 100%;
  overflow-x: scroll;
  white-space: nowrap;
}
.follow-items-wrapper {
  display: flex;
  flex-direction: row;
}
.follow-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100rpx;
  flex-shrink: 0;
  margin-right: 24rpx;
}
.follow-avatar {
  width: 100rpx;
  height: 100rpx;
  border-radius: 50%;
  object-fit: cover;
}
.follow-name {
  font-size: 24rpx;
  margin-top: 8rpx;
  text-align: center;
  white-space: nowrap;
}

/* 成就展示 */
.badge-list {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 20rpx;
  gap: 20rpx;
}

.badge-item {
  position: relative;
  width: 90rpx;
  height: 90rpx;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 4rpx 10rpx rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.badge-item:hover {
  transform: scale(1.1);
  box-shadow: 0 8rpx 16rpx rgba(0, 0, 0, 0.2);
}

.badge-icon {
  width: 60rpx;
  height: 60rpx;
  object-fit: contain;
}

.badge-label {
  font-size: 22rpx;
  color: #333;
  margin-top: 12rpx;
  text-align: center;
}

/* 功能入口 */
.menu {
  flex-direction: row;
  flex-wrap: wrap;
  display: flex;
  padding: 0 20rpx 30rpx;
  margin-top: 20rpx;
  justify-content: space-between;
}

.menu-item {
  width: 23%;
  background-color: #fff;
  margin-bottom: 20rpx;
  padding: 30rpx 0;
  border-radius: 16rpx;
  align-items: center;
  display: flex;
  flex-direction: column;
  box-shadow: 0 2rpx 4rpx rgba(0, 0, 0, 0.05);
}

.menu-icon {
  width: 80rpx;
  height: 80rpx;
  margin-bottom: 20rpx;
}

.menu-text {
  font-size: 28rpx;
  color: #333;
}
</style>