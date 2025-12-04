<template>
  <view class="container">
    <!-- 标题 -->
    <text class="title">热量摄入记录</text>

    <!-- 用户自定义目标热量 -->
    <view class="target-kcal-container">
      <text>今日目标摄入热量：</text>
      <input 
        type="number" 
        class="target-input" 
        placeholder="请输入目标热量(kcal)" 
        v-model.number="targetKcal"
        @blur="saveTargetKcal"
      />
    </view>

    <!-- 进度条 -->
    <view class="progress-bar-outer">
      <view 
        class="progress-bar-inner" 
        :style="{ width: progressPercent + '%' }"
        :class="{ 'over-target': progressPercent > 100 }"
      ></view>
    </view>

    <view class="progress-percent-text">
      {{ progressPercent }}%
    </view>

    <!-- 总热量显示 -->
    <view class="total-kcal-box">
      <text>今日已摄入热量：{{ totalKcal }} kcal</text>
    </view>

    <!-- 食物列表 -->
    <view v-for="(container, index) in containers" :key="index" class="food-container">
      <view class="circle-container" @click="uploadImage(index)">
        <text v-if="!container.selectedImage" class="plus-icon">+</text>
        <image v-else :src="container.selectedImage" mode="aspectFill" class="selected-image"></image>
      </view>
      <view class="inputs-container">
        <input type="text" class="foodname" placeholder="食物名称" v-model="container.item.name">
        <view class="row-inputs">
          <input type="text" class="foodweight" placeholder="质量(g)/容量(ml)" v-model="container.item.weight">
          <input type="text" class="foodkcal" placeholder="热量(kcal)" v-model="container.item.kcal">
        </view>
      </view>
    </view>

    <!-- 添加更多按钮 -->
    <view class="add-more-container" @click="addNewContainer">
      <text class="add-more-icon">+</text>
      <text class="add-more-text">添加食物</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      targetKcal: uni.getStorageSync('targetKcal') || 0,
      containers: uni.getStorageSync('containers') || [{
        selectedImage: '',
        item: {
          name: '',
          weight: '',
          kcal: ''
        }
      }]
    };
  },
  computed: {
    totalKcal() {
      return this.containers.reduce((sum, container) => {
        const kcal = parseFloat(container.item.kcal);
        return sum + (isNaN(kcal) ? 0 : kcal);
      }, 0);
    },
    progressPercent() {
      if (this.targetKcal <= 0) return 0;
      const percent = (this.totalKcal / this.targetKcal) * 100;
      return Math.min(120, Math.round(percent));
    }
  },
  methods: {
    uploadImage(index) {
      uni.chooseImage({
        count: 1,
        sourceType: ['camera', 'album'],
        success: (res) => {
          const tempFilePath = res.tempFilePaths[0];
          this.$set(this.containers[index], 'selectedImage', tempFilePath);
          this.saveData();
        },
        fail: (err) => {
          console.error('图片选择失败:', err);
        }
      });
    },
    addNewContainer() {
      this.containers.push({
        selectedImage: '',
        item: {
          name: '',
          weight: '',
          kcal: ''
        }
      });
      this.saveData();
    },
    saveData() {
      uni.setStorageSync('containers', this.containers);
    },
    saveTargetKcal() {
      uni.setStorageSync('targetKcal', this.targetKcal);
    }
  },
  onShow() {
    this.containers = uni.getStorageSync('containers') || [{
      selectedImage: '',
      item: {
        name: '',
        weight: '',
        kcal: ''
      }
    }];
    this.targetKcal = uni.getStorageSync('targetKcal') || 0;
  }
};
</script>

<style>
	.container {
	  padding: 30rpx;
	  background-color: #f8f9fa;
	}
	
	.title {
	  font-size: 40rpx;
	  font-weight: bold;
	  color: #333;
	  margin-bottom: 40rpx;
	  display: block;
	  text-align: center;
	}
	
	/* 目标热量输入框 */
	.target-kcal-container {
	  margin-bottom: 20rpx;
	}
	.target-kcal-container text {
	  font-size: 28rpx;
	  color: #555;
	  display: block;
	  margin-bottom: 10rpx;
	}
	.target-input {
	  width: 92%;
	  height: 70rpx;
	  line-height: 70rpx;
	  border: 1px solid #a5d6a7;
	  border-radius: 8rpx;
	  padding: 0 20rpx;
	  font-size: 28rpx;
	  background-color: #e8f5e9;
	  color: #2e7d32;
	}
	
	/* 进度条外层容器 */
	.progress-bar-outer {
	  width: 100%;
	  height: 24rpx;
	  background-color: #e0e0e0;
	  border-radius: 12rpx;
	  overflow: hidden;
	  margin-bottom: 10rpx;
	}
	
	/* 进度条内层 */
	.progress-bar-inner {
	  height: 100%;
	  background-color: #66bb6a;
	  transition: width 0.4s ease-in-out;
	  box-shadow: 0 2rpx 4rpx rgba(0, 150, 136, 0.2);
	}
	
	/* 超过目标时变红 */
	.progress-bar-inner.over-target {
	  background-color: #ef5350;
	}
	
	/* 百分比文字 */
	.progress-percent-text {
	  text-align: right;
	  font-size: 26rpx;
	  color: #666;
	  margin-bottom: 30rpx;
	}
	
	/* 总热量显示框 */
	.total-kcal-box {
	  background-color: #fff3cd;
	  border: 1px solid #ffeeba;
	  color: #856404;
	  padding: 24rpx;
	  margin-bottom: 30rpx;
	  font-size: 32rpx;
	  border-radius: 12rpx;
	  text-align: center;
	  box-shadow: 0 2rpx 6rpx rgba(0, 0, 0, 0.05);
	}
	
	/* 每个食物项 */
	.food-container {
	  display: flex;
	  align-items: center;
	  margin-bottom: 30rpx;
	  background-color: #ffffff;
	  border-radius: 16rpx;
	  box-shadow: 0 4rpx 10rpx rgba(0, 0, 0, 0.08);
	  padding: 24rpx;
	}
	
	/* 图片上传圆圈区域 */
	.circle-container {
	  width: 100rpx;
	  height: 100rpx;
	  border-radius: 50%;
	  background-color: #f1f1f1;
	  display: flex;
	  justify-content: center;
	  align-items: center;
	  border: 2rpx dashed #ccc;
	  margin-right: 24rpx;
	  position: relative;
	  transition: all 0.3s ease;
	}
	.circle-container:hover {
	  transform: scale(1.05);
	}
	
	/* 加号图标 */
	.plus-icon {
	  font-size: 40rpx;
	  color: #aaa;
	}
	
	/* 已选图片 */
	.selected-image {
	  width: 100%;
	  height: 100%;
	  border-radius: 50%;
	  object-fit: cover;
	  border: 2rpx solid #ddd;
	}
	
	/* 输入框区域 */
	.inputs-container {
	  width: 100%;
	  display: flex;
	  flex-direction: column;
	}
	
	/* 食物名称输入框 */
	.foodname {
	  width: 92%;
	  height: 70rpx;
	  line-height: 70rpx;
	  border: 1px solid #ddd;
	  border-radius: 8rpx;
	  margin-bottom: 16rpx;
	  padding: 0 20rpx;
	  font-size: 28rpx;
	  background-color: #fafafa;
	}
	
	/* 行输入布局 */
	.row-inputs {
	  display: flex;
	  justify-content: space-between;
	}
	
	/* 质量/热量输入框 */
	.foodweight, .foodkcal {
	  width: 48%;
	  height: 70rpx;
	  line-height: 70rpx;
	  border: 1px solid #ddd;
	  border-radius: 8rpx;
	  padding: 0 20rpx;
	  font-size: 28rpx;
	  background-color: #fafafa;
	}
	
	/* 添加按钮容器 */
	.add-more-container {
	  display: flex;
	  align-items: center;
	  justify-content: center;
	  margin-top: 40rpx;
	  background-color: #4caf50;
	  color: white;
	  padding: 20rpx 30rpx;
	  border-radius: 12rpx;
	  box-shadow: 0 4rpx 10rpx rgba(76, 175, 80, 0.3);
	  transition: background-color 0.3s;
	}
	.add-more-container:hover {
	  background-color: #43a047;
	}
	
	/* 添加按钮图标 */
	.add-more-icon {
	  font-size: 40rpx;
	  margin-right: 16rpx;
	}
	
	/* 添加按钮文字 */
	.add-more-text {
	  font-size: 32rpx;
	  font-weight: bold;
	}
</style>
