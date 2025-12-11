<template>
  <view class="container">
    <view class="header">
      <text class="title">历史趋势</text>
    </view>

    <view class="content">
      <!-- 近七日运动时长 -->
      <view class="icon-container">
        <image class="icon" src="/static/running.png" mode="aspectFit" />
        <text class="label">近七日运动时长概览 (分钟)</text>
      </view>
      <view class="chart-container">
        <text class="chart-title">近七日运动时长概览 (分钟)</text>
        <canvas canvas-id="weeklyChart" style="width: 300px; height: 200px;" />
      </view>

      <!-- 最近一个月每周运动时长 -->
      <view class="icon-container">
        <image class="icon" src="/static/monthly.png" mode="aspectFit" />
        <text class="label">月度运动时长周变化分析 (小时)</text>
      </view>
      <view class="chart-container">
        <text class="chart-title">月度运动时长周变化分析 (小时)</text>
        <canvas canvas-id="monthlyChart" style="width: 300px; height: 200px;" />
      </view>

      <!-- 近七日运动步数概览 -->
      <view class="icon-container">
        <image class="icon" src="/static/steps.png" mode="aspectFit" />
        <text class="label">近七日运动步数概览 (步)</text>
      </view>
      <view class="chart-container">
        <text class="chart-title">近七日运动步数概览 (步)</text>
        <canvas canvas-id="stepsChart" style="width: 300px; height: 200px;" />
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      weekData: [30, 45, 60, 25, 90, 120, 70],
      days: ['周一', '周二', '周三', '周四', '周五', '周六', '周日'],
      monthData: [1.2, 1.5, 1.0, 1.8], // 近一个月4周的运动时长（小时）
      monthWeeks: ['第1周', '第2周', '第3周', '第4周'],
      stepsData: [8000, 12000, 15000, 9000, 20000, 17000, 6000], // 近七日步数
      chartWidth: 300,
      chartHeight: 200
    };
  },
  onReady() {
    this.drawWeeklyChart();
    this.drawMonthlyChart();
    this.drawStepsChart();
  },
  methods: {
    drawWeeklyChart() {
      const ctx = uni.createCanvasContext('weeklyChart', this);
      const { chartWidth: width, chartHeight: height, days, weekData } = this;
      const padding = { top: 30, right: 15, bottom: 40, left: 40 };
      ctx.clearRect(0, 0, width, height);
      const maxValue = Math.max(...weekData) * 1.2;
      const xStep = (width - padding.left - padding.right) / (days.length - 1);
      const yStep = (height - padding.top - padding.bottom) / maxValue;
      // grid
      ctx.setStrokeStyle('#e0e0e0'); ctx.setLineWidth(1); ctx.beginPath();
      for (let i = 0; i <= 5; i++) {
        const y = padding.top + i * (height - padding.top - padding.bottom) / 5;
        ctx.moveTo(padding.left, y); ctx.lineTo(width - padding.right, y);
      }
      days.forEach((_, i) => {
        const x = padding.left + i * xStep;
        ctx.moveTo(x, padding.top); ctx.lineTo(x, height - padding.bottom);
      });
      ctx.stroke();
      // axes
      ctx.setStrokeStyle('#333'); ctx.setLineWidth(2); ctx.beginPath();
      ctx.moveTo(padding.left, padding.top); ctx.lineTo(padding.left, height - padding.bottom);
      ctx.moveTo(padding.left, height - padding.bottom); ctx.lineTo(width - padding.right, height - padding.bottom);
      ctx.stroke();
      // y labels
      ctx.setFontSize(10); ctx.setFillStyle('#666'); ctx.textAlign = 'right'; ctx.textBaseline = 'middle';
      for (let i = 0; i <= 5; i++) {
        const value = Math.round(maxValue * i / 5);
        const y = height - padding.bottom - i * (height - padding.top - padding.bottom) / 5;
        ctx.fillText(value.toString(), padding.left - 5, y);
      }
      // x labels
      ctx.setFontSize(10); ctx.textAlign = 'center'; ctx.textBaseline = 'top';
      days.forEach((d, i) => {
        const x = padding.left + i * xStep;
        ctx.fillText(d, x, height - padding.bottom + 8);
      });
      // line
      const pointsW = weekData.map((v, i) => ({ x: padding.left + i * xStep, y: height - padding.bottom - v * yStep }));
      ctx.setStrokeStyle('#42A5F5'); ctx.setLineWidth(2); ctx.beginPath();
      pointsW.forEach((p, i) => i === 0 ? ctx.moveTo(p.x, p.y) : ctx.lineTo(p.x, p.y)); ctx.stroke();
      // markers
      ctx.setFillStyle('#fff'); ctx.setStrokeStyle('#42A5F5'); ctx.setLineWidth(1.5);
      pointsW.forEach(p => { ctx.beginPath(); ctx.arc(p.x, p.y, 4, 0, 2 * Math.PI); ctx.fill(); ctx.stroke(); });
      // values
      ctx.setFontSize(12); ctx.setFillStyle('#42A5F5'); ctx.textBaseline = 'bottom';
      pointsW.forEach((p, i) => ctx.fillText(weekData[i].toString(), p.x, p.y - 8));
      // title
      ctx.setFontSize(14); ctx.setFillStyle('#333'); ctx.textAlign = 'center';
      ctx.fillText('近七日运动时长', width / 2, 20);
      ctx.draw();
    },
    drawMonthlyChart() {
      const ctx = uni.createCanvasContext('monthlyChart', this);
      const { chartWidth: width, chartHeight: height, monthWeeks, monthData } = this;
      const padding = { top: 30, right: 15, bottom: 40, left: 40 };
      ctx.clearRect(0, 0, width, height);
      const maxValue = Math.max(...monthData) * 1.2;
      const xStep = (width - padding.left - padding.right) / (monthWeeks.length - 1);
      const yStep = (height - padding.top - padding.bottom) / maxValue;
      // grid & axes\ n      ctx.setStrokeStyle('#e0e0e0'); ctx.setLineWidth(1); ctx.beginPath();
      for (let i=0;i<=5;i++){const y=padding.top+i*(height-padding.top-padding.bottom)/5;ctx.moveTo(padding.left,y);ctx.lineTo(width-padding.right,y);} 
      monthWeeks.forEach((_,i)=>{const x=padding.left+i*xStep;ctx.moveTo(x,padding.top);ctx.lineTo(x,height-padding.bottom);} ); ctx.stroke();
      ctx.setStrokeStyle('#333');ctx.setLineWidth(2);ctx.beginPath();
      ctx.moveTo(padding.left,padding.top);ctx.lineTo(padding.left,height-padding.bottom);
      ctx.moveTo(padding.left,height-padding.bottom);ctx.lineTo(width-padding.right,height-padding.bottom);
      ctx.stroke();
      // y labels\ n      ctx.setFontSize(10);ctx.setFillStyle('#666');ctx.textAlign='right';ctx.textBaseline='middle';
      for(let i=0;i<=5;i++){const value=(maxValue*i/5).toFixed(0);const y=height-padding.bottom-i*(height-padding.top-padding.bottom)/5;ctx.fillText(value.toString(),padding.left-5,y);} 
      // x labels
      ctx.setFontSize(10);ctx.textAlign='center';ctx.textBaseline='top';
      monthWeeks.forEach((d,i)=>{const x=padding.left+i*xStep;ctx.fillText(d,x,height-padding.bottom+8);} );
      // line
      const pointsM=monthData.map((v,i)=>({x:padding.left+i*xStep,y:height-padding.bottom-v*yStep}));
      ctx.setStrokeStyle('#ff7043');ctx.setLineWidth(2);ctx.beginPath();
      pointsM.forEach((p,i)=>i===0?ctx.moveTo(p.x,p.y):ctx.lineTo(p.x,p.y));ctx.stroke();
      // markers
      ctx.setFillStyle('#fff');ctx.setStrokeStyle('#ff7043');ctx.setLineWidth(1.5);
      pointsM.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,4,0,2*Math.PI);ctx.fill();ctx.stroke();});
      // values
      ctx.setFontSize(12);ctx.setFillStyle('#ff7043');ctx.textBaseline='bottom';
      pointsM.forEach((p,i)=>ctx.fillText(monthData[i].toString(),p.x,p.y-8));
      // title
      ctx.setFontSize(14);ctx.setFillStyle('#333');ctx.textAlign='center';
      ctx.fillText('月度运动时长周变化分析',width/2,20);
      ctx.draw();
    },
    drawStepsChart() {
      const ctx = uni.createCanvasContext('stepsChart', this);
      const { chartWidth: width, chartHeight: height, days, stepsData } = this;
      const padding = { top: 30, right: 15, bottom: 40, left: 40 };
      ctx.clearRect(0, 0, width, height);
      const maxValue = Math.max(...stepsData) * 1.2;
      const xStep = (width - padding.left - padding.right) / (days.length - 1);
      const yStep = (height - padding.top - padding.bottom) / maxValue;
      // grid & axes
      ctx.setStrokeStyle('#e0e0e0'); ctx.setLineWidth(1); ctx.beginPath();
      for(let i=0;i<=5;i++){const y=padding.top+i*(height-padding.top-padding.bottom)/5;ctx.moveTo(padding.left,y);ctx.lineTo(width-padding.right,y);} 
      days.forEach((_,i)=>{const x=padding.left+i*xStep;ctx.moveTo(x,padding.top);ctx.lineTo(x,height-padding.bottom);} );ctx.stroke();
      ctx.setStrokeStyle('#333');ctx.setLineWidth(2);ctx.beginPath();
      ctx.moveTo(padding.left,padding.top);ctx.lineTo(padding.left,height-padding.bottom);
      ctx.moveTo(padding.left,height-padding.bottom);ctx.lineTo(width-padding.right,height-padding.bottom);
      ctx.stroke();
      // y labels
      ctx.setFontSize(10);ctx.setFillStyle('#666');ctx.textAlign='right';ctx.textBaseline='middle';
      for(let i=0;i<=5;i++){const value=Math.round(maxValue*i/5);const y=height-padding.bottom-i*(height-padding.top-padding.bottom)/5;ctx.fillText(value.toString(),padding.left-5,y);} 
      // x labels
      ctx.setFontSize(10);ctx.textAlign='center';ctx.textBaseline='top';
      days.forEach((d,i)=>{const x=padding.left+i*xStep;ctx.fillText(d,x,height-padding.bottom+8);} );
      // line
      const pointsS=stepsData.map((v,i)=>({x:padding.left+i*xStep,y:height-padding.bottom-v*yStep}));
      ctx.setStrokeStyle('#66bb6a');ctx.setLineWidth(2);ctx.beginPath();
      pointsS.forEach((p,i)=>i===0?ctx.moveTo(p.x,p.y):ctx.lineTo(p.x,p.y));ctx.stroke();
      // markers
      ctx.setFillStyle('#fff');ctx.setStrokeStyle('#66bb6a');ctx.setLineWidth(1.5);
      pointsS.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,4,0,2*Math.PI);ctx.fill();ctx.stroke();});
      // values
      ctx.setFontSize(12);ctx.setFillStyle('#66bb6a');ctx.textBaseline='bottom';
      pointsS.forEach((p,i)=>ctx.fillText(stepsData[i].toString(),p.x,p.y-8));
      // title
      ctx.setFontSize(14);ctx.setFillStyle('#333');ctx.textAlign='center';
      ctx.fillText('近七日运动步数概览',width/2,20);
      ctx.draw();
    }
  }
};
</script>

<style scoped>
.container {
  padding: 20px;
  background-color: #eef2f5;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.header {
  width: 100%;
  padding: 16px 0;
  text-align: center;
  background: linear-gradient(90deg, #42A5F5 0%, #478ed1 100%);
  border-radius: 10px;
  margin-bottom: 24px;
}
.title {
  font-size: 28px;
  font-weight: 700;
  color: #ffffff;
}
.content {
  width: 100%;
  max-width: 340px;
  background-color: #ffffff;
  border-radius: 12px;
  padding: 16px;
}
.icon-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 16px;
  margin-bottom: 8px;
}
.icon {
  width: 60px;
  height: 60px;
  margin-bottom: 8px;
}
.label {
  font-size: 18px;
  font-weight: 600;
  color: #42A5F5;
}
.chart-container {
  background-color: #f7f9fc;
  border-radius: 10px;
  padding: 12px;
  margin-bottom: 20px;
}
.chart-title {
  text-align: center;
  margin-bottom: 8px;
  font-size: 16px;
  color: #333;
  font-weight: 600;
}
</style>
