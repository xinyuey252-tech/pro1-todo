<template>
  <view class="uni-margin-wrap">
    <swiper
      :indicator-dots="indicatorDots"
      :autoplay="autoplay"
      :interval="interval"
      :duration="duration"
      class="swiper"
    >
      <swiper-item
        v-for="(item, index) in healthTips"
        :key="index"
        @click="navigateToDetail(item.id)"
        class="swiper-item"
      >
        <!-- 使用 image 组件渲染背景图 -->
        <image
          class="swiper-bg"
          :src="item.imageUrl"
          mode="aspectFill"
        />
        <view class="overlay">
          <text class="title">{{ item.title }}</text>
          <text class="description">{{ item.description }}</text>
        </view>
      </swiper-item>
    </swiper>

    <!-- 健康建议分组 -->
    <view
      class="tip-section"
      v-for="(group, index) in tipGroups"
      :key="index"
    >
      <text class="tip-category">{{ group.category }}</text>
      <view
        class="tip-item"
        v-for="(tip, i) in group.tips"
        :key="i"
      >
        <text class="dot">●</text>
        <text class="tip-text">{{ tip }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      indicatorDots: true,
      autoplay: true,
      interval: 3000,
      duration: 500,
      healthTips: [
        {
          id: 1,
          title: '保持身体活动',
          description: '每天至少进行30分钟中等强度的运动。',
          imageUrl: '/static/recommend1.jpg'
        },
        {
          id: 2,
          title: '均衡饮食',
          description: '摄入各种颜色的新鲜水果和蔬菜。',
          imageUrl: '/static/recommend2.jpg'
        },
        {
          id: 3,
          title: '充足睡眠',
          description: '成年人每晚需要7到9小时的优质睡眠。',
          imageUrl: '/static/recommend3.jpg'
        }
      ],
      tipGroups: [
        {
          category: '🥗 饮食类',
          tips: [
            '多样饮食：每天摄入多种食物，确保获取足够维生素和矿物质。',
            '少盐控糖：成人每日盐摄入量不超过5克，尽量减少含糖饮料。',
            '高纤维摄入：多吃全谷物、豆类和蔬果，有助于肠道健康。',
            '合理饮水：每天饮水1500~2000毫升，首选温水或白开水。',
            '少吃加工食品：避免高油高盐高糖食品，减少慢病风险。'
          ]
        },
        {
          category: '🏃‍♂️ 运动类',
          tips: [
            '坚持有氧运动：如快走、游泳、骑行，每周至少150分钟。',
            '避免久坐：每坐1小时应起身活动5~10分钟，促进血液循环。',
            '力量训练：每周进行2次抗阻训练，增强肌肉和骨骼。',
            '热身与拉伸：运动前热身，运动后拉伸，预防运动损伤。',
            '适应气候运动：寒暑适中时更适合户外锻炼，注意补水和防晒。'
          ]
        },
        {
          category: '😴 睡眠类',
          tips: [
            '规律作息：保持每天固定时间睡觉与起床，培养生物钟。',
            '避免熬夜：建议23点前入睡，有助于身体修复与免疫力提升。',
            '睡前放松：可做深呼吸或听轻音乐，帮助身心进入睡眠状态。',
            '营造良好环境：卧室安静、光线柔和、温度适宜更利于睡眠。',
            '远离电子屏幕：睡前1小时不看手机、电脑，避免蓝光干扰。'
          ]
        },
        {
          category: '💬 心理类',
          tips: [
            '学会减压：冥想、运动、写日记等都是释放压力的好方法。',
            '保持积极心态：面对困难保持乐观有助于增强心理弹性。',
            '社交互动：与亲友多沟通，减少孤独感，提升幸福感。',
            '设定小目标：逐步完成有助于提升自信心和掌控感。',
            '寻求帮助：情绪长期低落或焦虑应及时寻求心理咨询支持。'
          ]
        }
      ]
    };
  },
  methods: {
    navigateToDetail(id) {
      let url = '';
      switch (id) {
        case 1:
          url = '/pages/recommendpage1/recommendpage1';
          break;
        case 2:
          url = '/pages/recommendpage2/recommendpage2';
          break;
        case 3:
          url = '/pages/recommendpage3/recommendpage3';
          break;
        default:
          url = `/pages/detail/detail?id=${id}`;
      }
      uni.navigateTo({ url });
    }
  }
};
</script>

<style scoped>
.uni-margin-wrap {
  width: 100%;
  background-color: #f6fff8;
}
.swiper {
  height: 320rpx;
}
.swiper-item {
  position: relative;
  border-radius: 16rpx;
  overflow: hidden;
  box-shadow: 0 4rpx 12rpx rgba(0, 128, 0, 0.15);
  height: 320rpx;
}

.swiper-bg {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 95%;
  height: 95%;
  object-fit: cover; /* 确保图片覆盖整个区域 */
}

.overlay {
  position: absolute;
  bottom: 0;
  width: 100%;
  background: linear-gradient(to top, rgba(0, 128, 0, 0.5), rgba(0, 128, 0, 0));
  padding: 30rpx;
}
.title {
  font-size: 40rpx;
  font-weight: bold;
  color: #ffffff;
  margin-bottom: 10rpx;
}
.description {
  font-size: 30rpx;
  color: #e6ffe6;
  line-height: 1.6;
}
.tip-section {
  padding: 40rpx 30rpx;
  border-bottom: 1px solid #e0f2e9;
  background-color: #ffffff;
  border-radius: 12rpx;
  margin: 20rpx;
  box-shadow: 0 4rpx 10rpx rgba(0, 128, 0, 0.08);
}
.tip-category {
  font-size: 36rpx;
  font-weight: bold;
  color: #2e7d32;
  margin-bottom: 20rpx;
  border-left: 8rpx solid #66bb6a;
  padding-left: 20rpx;
}
.tip-item {
  display: flex;
  align-items: flex-start;
  font-size: 30rpx;
  color: #4d774e;
  margin-bottom: 16rpx;
  line-height: 1.8;
}
.dot {
  color: #66bb6a;
  margin-right: 12rpx;
  font-size: 28rpx;
}
.tip-text {
  flex: 1;
}
</style>
