<template>
	<view class="container">
	  <!-- 搜索框 -->
	  <view class="search-box">
	    <input placeholder="输入食物名称" bindinput="onInput" value="{{searchText}}" />
	  </view>
	
	  <!-- 食物分类选择器 -->
	  <picker mode="selector" range="{{foodTypes}}" range-key="name" bindchange="onChangeType">
	    <view class="picker">选择分类：{{selectedType}}</view>
	  </picker>
	
	  <!-- 食物列表 -->
	  <block wx:for="{{filteredFoods}}" wx:key="name">
	    <view bindtap="selectFood" data-food="{{item}}">
	      {{item.name}} - {{item.calories}}大卡 /100g
	    </view>
	  </block>
	
	  <!-- 输入份量 -->
	  <view class="amount-input">
	    <input type="number" placeholder="请输入克数" bindinput="onAmountChange" value="{{amount}}" />
	  </view>
	
	  <!-- 显示计算结果 -->
	  <view wx:if="{{selectedFood}}">
	    <text>你选择了：{{selectedFood.name}}（{{amount}}克）</text>
	    <text>热量：{{totalCalories}} 大卡</text>
	    <text>蛋白质：{{totalProtein}} 克</text>
	    <text>脂肪：{{totalFat}} 克</text>
	    <text>碳水：{{totalCarbs}} 克</text>
	  </view>
	</view>
</template>

<script>
	const foodList = require('../../utils/foodData.js');
	
	export default{
	  data() {
		  return{
			searchText: '',
			amount: 100,
			selectedFood: null,
			foodList: foodList,
			filteredFoods: foodList,
			foodTypes: [
			  { name: '全部' },
			  { name: '蔬菜' },
			  { name: '水果' },
			  { name: '肉类' },
			  { name: '主食' },
			  { name: '乳制品' }
			],
			selectedType: '全部'
			};
		  },
	
	  onInput(e) {
	    const text = e.detail.value;
	    this.setData({
	      searchText: text,
	      filteredFoods: this.data.foodList.filter(food => food.name.includes(text))
	    });
	  },
	
	  onChangeType(e) {
	    const index = e.detail.value;
	    const type = this.data.foodTypes[index].name;
	    let list = this.data.foodList;
	
	    if (type !== '全部') {
	      list = this.data.foodList.filter(food => food.type === type);
	    }
	
	    this.setData({
	      selectedType: type,
	      filteredFoods: list
	    });
	  },
	
	  selectFood(e) {
	    const food = e.currentTarget.dataset.food;
	    this.setData({ selectedFood: food });
	    this.calculateNutrition(food);
	  },
	
	  onAmountChange(e) {
	    const amount = parseInt(e.detail.value) || 100;
	    this.setData({ amount });
	    if (this.data.selectedFood) {
	      this.calculateNutrition(this.data.selectedFood);
	    }
	  },
	
	  calculateNutrition(food) {
	    const amount = this.data.amount;
	    const factor = amount / 100;
	    this.setData({
	      totalCalories: (food.calories * factor).toFixed(2),
	      totalProtein: (food.protein * factor).toFixed(2),
	      totalFat: (food.fat * factor).toFixed(2),
	      totalCarbs: (food.carbs * factor).toFixed(2)
	    });
	  }
	}
</script>

<style>
	.container {
	  padding: 20rpx;
	}
	input {
	  border: 1rpx solid #ccc;
	  margin-bottom: 20rpx;
	  padding: 20rpx;
	}
	.picker {
	  margin-bottom: 20rpx;
	  padding: 20rpx;
	  background-color: #f0f0f0;
	}
</style>