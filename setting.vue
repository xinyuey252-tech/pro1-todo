<template>
  <view class="container">
    <!-- 性别选择 -->
    <view class="section card">
      <text class="label">性别</text>
      <radio-group @change="onGenderChange">
        <view class="radio-options">
          <label v-for="item in genderOptions" :key="item.value" class="radio-option">
            <radio :value="item.value" :checked="gender === item.value" />
            <text>{{ item.label }}</text>
          </label>
        </view>
      </radio-group>
    </view>

    <!-- 运动偏好 -->
    <view class="section card">
      <text class="label">运动偏好</text>
      <checkbox-group @change="onHobbyChange">
        <view class="checkbox-options">
          <label v-for="item in hobbyOptions" :key="item.value" class="checkbox-option">
            <checkbox :value="item.value" :checked="hobbies.includes(item.value)" />
            <text>{{ item.label }}</text>
          </label>
        </view>
      </checkbox-group>
      <view v-if="hobbies.includes('custom')" class="input-group">
        <input v-model="customHobby" class="custom-input" placeholder="请输入自定义偏好" />
      </view>
    </view>

    <!-- 生日选择 -->
    <view class="section card">
      <text class="label">生日</text>
      <picker mode="date" :value="birthday" @change="onBirthdayChange">
        <view class="picker">
          {{ birthday || '请选择生日' }}
        </view>
      </picker>
    </view>

    <!-- 身高、体重、身体维度 -->
    <view class="section card">
      <text class="label">身高（cm）</text>
      <input v-model="height" type="number" class="input-field" placeholder="请输入身高" />

      <text class="label">体重（kg）</text>
      <input v-model="weight" type="number" class="input-field" placeholder="请输入体重" />

      <text class="label">腰围（cm）</text>
      <input v-model="waist" type="number" class="input-field" placeholder="请输入腰围" />

      <text class="label">臀围（cm）</text>
      <input v-model="hip" type="number" class="input-field" placeholder="请输入臀围" />

      <text class="label">胸围（cm）</text>
      <input v-model="chest" type="number" class="input-field" placeholder="请输入胸围" />
    </view>

    <!-- 保存按钮 -->
    <view class="section">
      <button class="save-btn" @click="saveSetting">保存设置</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      gender: '',
      genderOptions: [
        { label: '男', value: 'male' },
        { label: '女', value: 'female' },
        { label: '保密', value: 'secret' },
      ],
      hobbies: [],
      hobbyOptions: [
        { label: '零噪音', value: 'quiet' },
        { label: '手腕友好', value: 'wrist' },
        { label: '音乐踩点', value: 'music' },
        { label: '动感配乐', value: 'dynamic' },
        { label: '改善失眠', value: 'sleep' },
        { label: '自定义', value: 'custom' },
      ],
      customHobby: '',
      birthday: '',
      height: '',
      weight: '',
      waist: '',
      hip: '',
      chest: ''
    }
  },
  methods: {
    onGenderChange(e) {
      this.gender = e.detail.value;
    },
    onHobbyChange(e) {
      this.hobbies = e.detail.value;
    },
    onBirthdayChange(e) {
      this.birthday = e.detail.value;
    },
    saveSetting() {
      const settings = {
        gender: this.gender,
        hobbies: this.hobbies.includes('custom') ? [...this.hobbies.filter(h => h !== 'custom'), this.customHobby] : this.hobbies,
        birthday: this.birthday,
        height: this.height,
        weight: this.weight,
        waist: this.waist,
        hip: this.hip,
        chest: this.chest,
      };
      console.log('保存设置：', settings);
      uni.showToast({ title: '设置已保存', icon: 'success' });
    }
  }
}
</script>

<style scoped>
.container {
  padding: 30rpx;
  background: linear-gradient(to bottom right, #e0f2f1, #a5d6a7);
  min-height: 100vh;
}

/* 卡片样式 */
.card {
  background-color: #ffffffcc;
  border-radius: 20rpx;
  padding: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 128, 0, 0.1);
}

/* 标题样式 */
.label {
  font-size: 30rpx;
  font-weight: bold;
  margin-bottom: 10rpx;
  color: #2e7d32;
}

/* 输入框通用样式 */
.input-field, .custom-input {
  width: 100%;
  padding: 14rpx;
  border-radius: 12rpx;
  border: 1px solid #c8e6c9;
  background-color: #f1f8e9;
  font-size: 28rpx;
  margin-top: 10rpx;
  color: #33691e;
}

/* 自定义偏好输入框 */
.custom-input {
  margin-top: 20rpx;
}

/* 日期选择器 */
.picker {
  padding: 14rpx;
  background-color: #f1f8e9;
  border-radius: 12rpx;
  text-align: center;
  font-size: 28rpx;
  color: #33691e;
  border: 1px solid #c8e6c9;
  margin-top: 10rpx;
}

/* 按钮 */
.save-btn {
  width: 100%;
  background: linear-gradient(to right, #66bb6a, #43a047);
  color: #fff;
  padding: 14rpx 0;
  font-size: 28rpx;
  border-radius: 10rpx;
  text-align: center;
  margin-top: 30rpx;
  font-weight: 600;
  box-shadow: 0 4rpx 10rpx rgba(76, 175, 80, 0.25);
}


/* 单选组 */
.radio-options {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-top: 10rpx;
}
.radio-option {
  display: flex;
  align-items: center;
  color: #388e3c;
}

/* 多选组 */
.checkbox-options {
  display: flex;
  flex-wrap: wrap;
  gap: 20rpx;
  margin-top: 10rpx;
}
.checkbox-option {
  display: flex;
  align-items: center;
  color: #388e3c;
}

/* 间距控制 */
.section .input-field,
.section .save-btn {
  margin-top: 20rpx;
}
</style>
