<template>
  <scroll-view class="container" scroll-y :style="containerStyle">
    <!-- 成就展示 -->
    <view class="section">
      <text class="section-title">成就展示</text>

      <!-- 奖项内容列表 -->
      <view class="badge-list">
        <!-- 奖项分类 -->
        <view v-for="(items, index) in badgeCategories" :key="index" class="badge-category">
          <view class="badge-title">{{ items.title }}</view>
          <view class="badge-item-list">
            <view
              v-for="badge in items.badges"
              :key="badge.name"
              class="badge-item"
              @tap="showBadgeDetail(badge.name)"
            >
              <image :src="badge.icon" class="badge-icon" mode="aspectFit" />
              <text class="badge-label">{{ badge.name }}</text>
            </view>
          </view>
        </view>
      </view>
    </view>

    <!-- 奖牌简介弹窗 -->
    <view v-if="showModal" class="modal">
      <view class="modal-content">
        <text class="modal-title">{{ selectedBadge }}</text>
        <text class="modal-description">{{ badgeDescriptions[selectedBadge] }}</text>
        <button class="close-btn" @tap="closeModal">关闭</button>
      </view>
    </view>
  </scroll-view>
</template>

<script>
export default {
  data() {
    return {
      showModal: false,
      selectedBadge: '',
      badgeCategories: [
        {
          title: '奔跑逐梦，律动人生',
          badges: [
            { name: '晨跑达人奖', icon: '/static/badge1.png' },
            { name: '坚持不懈奖', icon: '/static/badge2.png' },
            { name: '爆发力冠军奖', icon: '/static/badge3.png' },
            { name: '完美时刻奖', icon: '/static/badge4.png' },
          ],
        },
        {
          title: '骑行天地，畅享自由',
          badges: [
            { name: '骑行新手奖', icon: '/static/badgeknown.png' },
            { name: '骑行进阶奖', icon: '/static/badgeknown.png' },
            { name: '骑行达人奖', icon: '/static/badgeknown.png' },
            { name: '骑行大师奖', icon: '/static/badgeknown.png' },
          ],
        },
        {
          title: '燃动全身，重塑自我',
          badges: [
            { name: '燃脂起步奖', icon: '/static/badgeknown.png' },
            { name: '燃脂突破奖', icon: '/static/badgeknown.png' },
            { name: '燃脂先锋奖', icon: '/static/badgeknown.png' },
            { name: '燃脂王者奖', icon: '/static/badgeknown.png' },
          ],
        },
        {
          title: '健康改善，拥抱生活',
          badges: [
            { name: '健康觉醒奖', icon: '/static/badgeknown.png' },
            { name: '健康进步奖', icon: '/static/badgeknown.png' },
            { name: '健康标兵奖', icon: '/static/badgeknown.png' },
            { name: '健康大使奖', icon: '/static/badgeknown.png' },
          ],
        },
        {
          title: '力量训练，强韧自我',
          badges: [
            { name: '力量入门奖', icon: '/static/badgeknown.png' },
            { name: '力量提升奖', icon: '/static/badgeknown.png' },
            { name: '力量强者奖', icon: '/static/badgeknown.png' },
            { name: '力量大师奖', icon: '/static/badgeknown.png' },
          ],
        },
        {
          title: '新增主题奖项',
          badges: [
            { name: '奔跑逐梦奖', icon: '/static/badgeknown.png' },
            { name: '骑行天地奖', icon: '/static/badgeknown.png' },
            { name: '燃动全身奖', icon: '/static/badgeknown.png' },
            { name: '焕新健康奖', icon: '/static/badgeknown.png' },
            { name: '聚力前行奖', icon: '/static/badgeknown.png' },
          ],
        },
      ],
      badgeDescriptions: {
        '晨跑达人奖': `“晨跑达人奖”是为了表彰在清晨跑步中坚持不懈的你。你迎着晨曦、踏着露水，每一步都充满了活力与决心。无论天气如何变化，早晨的跑步已经成为了你生活的一部分。你用行动证明了，只要每天都坚持，逐渐积累的力量可以让你超越昨日的自己。你是早起者，也是梦想的追逐者！`,
        '坚持不懈奖': `“坚持不懈奖”是送给那些无论遇到多大的困难和挑战，依然坚守跑步计划的你。无论是突如其来的忙碌，还是偶尔的疲惫，你都从未放弃自己的目标。每一次的突破和进步，都离不开你那份始终如一的毅力。你教会了自己，只有通过坚持，才能真正看到胜利的曙光。`,
		'爆发力冠军奖': `“爆发力冠军奖”是专门为那些在某一段跑步中展现超强爆发力的运动者颁发的。这份奖牌代表了你在最关键的时刻，能够突破自我，挑战极限。在你的跑步生涯中，总有几次你像飞一样冲刺，那一刻的速度和力量，令你自己也为之惊叹。这份奖牌不仅仅是对你身体素质的肯定，更是对你不畏挑战、敢于突破的精神的奖赏。`,
		'完美时刻奖': `“完美时刻奖”是为那些在赛道上展现出极致完美跑步状态的你而设。这不仅仅是一次普通的跑步，更是一次身体与精神的完美融合。当你在那一刻跑得如风一般轻盈，每一步都准确无误地踩在节奏上，你感受到了与自己内心的深刻连接。这一瞬间，你的每一次呼吸、每一次摆臂都完美无缺。这个奖牌，是对你跑步技术和状态的终极肯定。`,
        
		'骑行新手奖': `“骑行新手奖” 是为那些刚刚踏上骑行之旅的你准备的。从你第一次跨上自行车，握住车把，尝试着蹬动踏板开始，你就勇敢地迈出了骑行的第一步。尽管最初可能会有些紧张，骑行的距离也不长，但你那份对骑行的热情和勇气已然值得肯定。每一次小小的前进，都是你在骑行世界里探索的见证。你是骑行新手，也是未来骑行之路的开拓者。`,
        '骑行进阶奖': `“骑行进阶奖” 是颁发给在骑行之路上不断进步的你。随着时间的推移，你逐渐掌握了更多的骑行技巧，能够更自如地操控自行车。你开始挑战更长的距离，穿越不同的地形，无论是平坦的公路还是稍有起伏的小道，都能看到你坚定骑行的身影。你在骑行中不断突破自我，每一次的进步都凝聚着你的努力和坚持。`,
		'骑行达人奖': `“骑行达人奖” 是授予在骑行领域展现出卓越能力的你。你已经不再满足于普通的骑行路线，而是敢于挑战高难度的骑行赛道。你对自行车的性能了如指掌，能够根据不同的路况做出最佳的骑行决策。你在骑行中展现出的耐力、速度和技巧，让周围的人都为之赞叹。你是骑行达人，用骑行诠释着自由与激情。`,
		'骑行大师奖': `“骑行大师奖” 是专门为那些在骑行方面达到顶尖水平的你设立的。你有着丰富的骑行经验和深厚的骑行知识，无论是长途骑行还是竞技骑行，你都能游刃有余。你不仅在骑行技术上炉火纯青，还能引领身边的人一起享受骑行的乐趣。你是骑行的领路人，你的骑行精神激励着更多的人加入这项运动。`,
        
		'燃脂起步奖': '“燃脂起步奖” 是为那些刚刚开启全身燃脂之旅的你而设。从你决定改变自己，开始进行燃脂运动的那一刻起，你就踏上了一条充满挑战与收获的道路...',
		'燃脂突破奖': '“燃脂突破奖” 是送给在全身燃脂过程中取得明显进步的你。经过一段时间的坚持，你逐渐适应了运动的强度，身体也开始发生变化...',
		'燃脂先锋奖': '“燃脂先锋奖” 是属于那些在全身燃脂方面表现出色的你。你不仅严格遵循着燃脂计划，还不断探索更高效的燃脂方法...',
		'燃脂王者奖': '“燃脂王者奖” 是为那些在全身燃脂领域达到巅峰的你颁发的。你已经成功塑造了健康、完美的身材，体脂率达到了理想的水平...',
		
		'健康觉醒奖': '“健康觉醒奖” 是为那些开始关注自身健康，决定做出改变的你准备的。当你意识到健康的重要性，开始调整生活方式...',
		'健康进步奖': '“健康进步奖” 是颁发给在健康改善过程中取得阶段性成果的你。通过一段时间的努力，你发现自己的身体状况有了明显的改善...',
		'健康标兵奖': '“健康标兵奖” 是授予在健康改善方面表现突出的你。你不仅自己坚持健康的生活方式，还积极影响身边的人...',
		'健康大使奖': '“健康大使奖” 是专门为那些在健康改善领域达到卓越水平，并且能够广泛传播健康理念的你设立的...',
		
		'力量入门奖': '“力量训练入门奖” 是为那些刚刚接触力量训练的你而设。从你第一次拿起哑铃，尝试进行简单的力量动作时，你就开启了力量训练的大门...',
		'力量提升奖': '“力量提升奖” 是送给在力量训练中取得显著进步的你。随着训练的深入，你逐渐掌握了正确的力量训练方法，肌肉力量和耐力都有了明显的提升...',
		'力量强者奖': '“力量强者奖” 是属于那些在力量训练领域展现出强大实力的你。你已经拥有了出色的肌肉力量和爆发力，能够轻松完成高难度的力量动作...',
		'力量大师奖': '“力量大师奖” 是为那些在力量训练方面达到顶尖水平的你颁发的。你不仅拥有令人惊叹的肌肉力量和完美的身材比例，还在力量训练领域有着深厚的造诣...',
		
      },
      containerStyle: {
        background: 'linear-gradient(135deg, #56ab2f, #a8e063)', // 绿色渐变背景
        backgroundSize: 'cover',
        minHeight: '100vh',
      },
    };
  },
  methods: {
    showBadgeDetail(badgeName) {
      this.selectedBadge = badgeName;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.selectedBadge = '';
    },
  },
};
</script>

<style scoped>
.container {
  background-size: cover;
}

.section-title {
  font-size: 32rpx;
  font-weight: bold;
  text-align: center;
  color: #fff;
  margin-bottom: 20rpx;
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.4);
}

.badge-category {
  margin-bottom: 40rpx;
}

.badge-title {
  font-size: 28rpx;
  font-weight: 600;
  color: #fff;
  margin-bottom: 15rpx;
  text-align: center;
}

.badge-item-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 16rpx;
  /* padding: 0 16rpx; */
}

.badge-item {
  width: 42%;
  height: 140rpx;
  /* margin: 20rpx; */
  margin-left: 20rpx;
  margin-right: 20rpx;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 12rpx;
  box-shadow: 0 12rpx 24rpx rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
  /* padding: 15rpx; */
}

.badge-item:hover {
  transform: scale(1.05);
  box-shadow: 0 10rpx 30rpx rgba(0, 0, 0, 0.2);
}

.badge-icon {
  width: 80rpx;
  height: 80rpx;
  margin-bottom: 10rpx;
}

.badge-label {
  font-size: 24rpx;
  color: #333;
  margin-top: 10rpx;
  font-weight: bold;
  text-align: center;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20rpx;
}

.modal-content {
  background-color: #fff;
  padding: 40rpx;
  border-radius: 20rpx;
  width: 80%;
  max-width: 600rpx;
  box-shadow: 0 12rpx 36rpx rgba(0, 0, 0, 0.1);
  animation: fadeIn 0.5s ease-out;
}

.modal-title {
  font-size: 36rpx;
  font-weight: bold;
  text-align: center;
  color: #333;
  margin-bottom: 20rpx;
}

.modal-description {
  font-size: 28rpx;
  color: #666;
  margin-bottom: 30rpx;
  text-align: justify;
}

.close-btn {
  background-color: #FF7043;
  color: #fff;
  padding: 15rpx;
  border: none;
  border-radius: 8rpx;
  cursor: pointer;
  font-size: 28rpx;
  width: 100%;
  text-align: center;
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
