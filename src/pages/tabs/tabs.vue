<template>
  <view custom-class="tabs-page">
    <press-tabs :active="activeTab" @change="onTabChange" :swipeable="false">
      <press-tab title="用户信息">
        <view custom-class="table-container">
          <view custom-class="table-wrapper">
            <press-row custom-class="table-header" :gutter="0">
              <press-col :span="4" custom-class="table-cell header-cell">姓名</press-col>
              <press-col :span="4" custom-class="table-cell header-cell">年龄</press-col>
              <press-col :span="4" custom-class="table-cell header-cell">性别</press-col>
              <press-col :span="4" custom-class="table-cell header-cell">职业</press-col>
              <press-col :span="4" custom-class="table-cell header-cell">城市</press-col>
              <press-col :span="4" custom-class="table-cell header-cell">状态</press-col>
            </press-row>
            <press-row v-for="(user, index) in userList" :key="index" custom-class="table-row" :gutter="0">
              <press-col :span="4" custom-class="table-cell">{{ user.name }}</press-col>
              <press-col :span="4" custom-class="table-cell">{{ user.age }}</press-col>
              <press-col :span="4" custom-class="table-cell">{{ user.gender }}</press-col>
              <press-col :span="4" custom-class="table-cell">{{ user.job }}</press-col>
              <press-col :span="4" custom-class="table-cell">{{ user.city }}</press-col>
              <press-col :span="4" custom-class="table-cell">{{ user.status }}</press-col>
            </press-row>
          </view>
        </view>
      </press-tab>
      
      <press-tab title="产品数据">
        <view custom-class="table-container">
          <view custom-class="table-wrapper">
            <press-row custom-class="table-header" :gutter="0">
              <press-col :span="3" custom-class="table-cell header-cell">产品名称</press-col>
              <press-col :span="3" custom-class="table-cell header-cell">价格</press-col>
              <press-col :span="3" custom-class="table-cell header-cell">库存</press-col>
              <press-col :span="3" custom-class="table-cell header-cell">分类</press-col>
              <press-col :span="3" custom-class="table-cell header-cell">品牌</press-col>
              <press-col :span="3" custom-class="table-cell header-cell">评分</press-col>
              <press-col :span="3" custom-class="table-cell header-cell">销量</press-col>
              <press-col :span="3" custom-class="table-cell header-cell">状态</press-col>
            </press-row>
            <press-row v-for="(product, index) in productList" :key="index" custom-class="table-row" :gutter="0">
              <press-col :span="3" custom-class="table-cell">{{ product.name }}</press-col>
              <press-col :span="3" custom-class="table-cell">¥{{ product.price }}</press-col>
              <press-col :span="3" custom-class="table-cell">{{ product.stock }}</press-col>
              <press-col :span="3" custom-class="table-cell">{{ product.category }}</press-col>
              <press-col :span="3" custom-class="table-cell">{{ product.brand }}</press-col>
              <press-col :span="3" custom-class="table-cell">{{ product.rating }}</press-col>
              <press-col :span="3" custom-class="table-cell">{{ product.sales }}</press-col>
              <press-col :span="3" custom-class="table-cell">{{ product.status }}</press-col>
            </press-row>
          </view>
        </view>
      </press-tab>
      
      <press-tab title="订单统计">
        <view custom-class="table-container">
          <view custom-class="table-wrapper">
            <press-row custom-class="table-header" :gutter="0">
              <press-col :span="4" custom-class="table-cell header-cell">订单号</press-col>
              <press-col :span="4" custom-class="table-cell header-cell">客户</press-col>
              <press-col :span="4" custom-class="table-cell header-cell">金额</press-col>
              <press-col :span="4" custom-class="table-cell header-cell">日期</press-col>
              <press-col :span="4" custom-class="table-cell header-cell">支付方式</press-col>
              <press-col :span="4" custom-class="table-cell header-cell">状态</press-col>
            </press-row>
            <press-row v-for="(order, index) in orderList" :key="index" custom-class="table-row" :gutter="0">
              <press-col :span="4" custom-class="table-cell">{{ order.orderNo }}</press-col>
              <press-col :span="4" custom-class="table-cell">{{ order.customer }}</press-col>
              <press-col :span="4" custom-class="table-cell">¥{{ order.amount }}</press-col>
              <press-col :span="4" custom-class="table-cell">{{ order.date }}</press-col>
              <press-col :span="4" custom-class="table-cell">{{ order.payMethod }}</press-col>
              <press-col :span="4" custom-class="table-cell">{{ order.status }}</press-col>
            </press-row>
          </view>
        </view>
      </press-tab>
    </press-tabs>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import PressTab from 'press-ui/press-tab/press-tab.vue'
import PressTabs from 'press-ui/press-tabs/press-tabs.vue'
import PressRow from 'press-ui/press-row/press-row.vue'
import PressCol from 'press-ui/press-col/press-col.vue'

// 当前激活的tab
const activeTab = ref(0)

// 用户信息数据
const userList = ref([
  { name: '张三', age: 28, gender: '男', job: '工程师', city: '北京', status: '在职' },
  { name: '李四', age: 32, gender: '女', job: '设计师', city: '上海', status: '在职' },
  { name: '王五', age: 25, gender: '男', job: '产品经理', city: '深圳', status: '离职' },
  { name: '赵六', age: 30, gender: '女', job: '运营', city: '广州', status: '在职' },
  { name: '钱七', age: 27, gender: '男', job: '测试', city: '杭州', status: '在职' },
  { name: '孙八', age: 35, gender: '女', job: '项目经理', city: '成都', status: '在职' },
  { name: '周九', age: 29, gender: '男', job: '架构师', city: '武汉', status: '在职' },
  { name: '吴十', age: 26, gender: '女', job: '前端开发', city: '西安', status: '在职' }
])

// 产品数据
const productList = ref([
  { name: 'iPhone 15', price: 5999, stock: 120, category: '手机', brand: '苹果', rating: 4.8, sales: 1580, status: '在售' },
  { name: 'MacBook Pro', price: 12999, stock: 45, category: '电脑', brand: '苹果', rating: 4.9, sales: 320, status: '在售' },
  { name: '小米13', price: 3999, stock: 200, category: '手机', brand: '小米', rating: 4.6, sales: 2100, status: '在售' },
  { name: 'ThinkPad X1', price: 8999, stock: 30, category: '电脑', brand: '联想', rating: 4.5, sales: 150, status: '在售' },
  { name: 'AirPods Pro', price: 1999, stock: 80, category: '耳机', brand: '苹果', rating: 4.7, sales: 890, status: '在售' },
  { name: '华为P60', price: 4999, stock: 0, category: '手机', brand: '华为', rating: 4.4, sales: 560, status: '缺货' },
  { name: 'Surface Pro', price: 7999, stock: 25, category: '电脑', brand: '微软', rating: 4.3, sales: 180, status: '在售' },
  { name: 'Galaxy S23', price: 5499, stock: 90, category: '手机', brand: '三星', rating: 4.5, sales: 720, status: '在售' }
])

// 订单数据
const orderList = ref([
  { orderNo: 'ORD001', customer: '张三', amount: 5999, date: '2024-01-15', payMethod: '支付宝', status: '已完成' },
  { orderNo: 'ORD002', customer: '李四', amount: 12999, date: '2024-01-16', payMethod: '微信支付', status: '已完成' },
  { orderNo: 'ORD003', customer: '王五', amount: 3999, date: '2024-01-17', payMethod: '银行卡', status: '处理中' },
  { orderNo: 'ORD004', customer: '赵六', amount: 1999, date: '2024-01-18', payMethod: '支付宝', status: '已完成' },
  { orderNo: 'ORD005', customer: '钱七', amount: 8999, date: '2024-01-19', payMethod: '微信支付', status: '已取消' },
  { orderNo: 'ORD006', customer: '孙八', amount: 4999, date: '2024-01-20', payMethod: '支付宝', status: '已完成' },
  { orderNo: 'ORD007', customer: '周九', amount: 7999, date: '2024-01-21', payMethod: '银行卡', status: '处理中' },
  { orderNo: 'ORD008', customer: '吴十', amount: 5499, date: '2024-01-22', payMethod: '微信支付', status: '已完成' }
])

// tab切换事件
function onTabChange(index: number) {
  activeTab.value = index
  console.log('切换到tab:', index)
}
</script>

<style>
.tabs-page {
  width: 100%;
  height: 100vh;
  background-color: #f8f8f8;
}

.table-container {
  padding: 20rpx;
  overflow-x: auto;
}

.table-wrapper {
  min-width: 100%;
  background-color: #fff;
  border-radius: 12rpx;
  overflow: hidden;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.1);
}

.table-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.table-row {
  border-bottom: 1rpx solid #f0f0f0;
  transition: background-color 0.2s ease;
}

.table-row:last-child {
  border-bottom: none;
}

.table-cell {
  padding: 24rpx 16rpx;
  text-align: center;
  font-size: 28rpx;
  line-height: 1.4;
  border-right: 1rpx solid #f0f0f0;
  min-height: 88rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  word-break: break-all;
  box-sizing: border-box;
}

.header-cell {
  background: transparent;
  color: #fff;
  font-weight: 600;
  font-size: 30rpx;
}

.table-cell:last-child {
  border-right: none;
}

/* 多端兼容样式优化 */
/* #ifdef H5 */
.table-row:hover {
  background-color: #f8f9ff;
}

.table-cell {
  font-size: 28rpx;
}
/* #endif */

/* #ifdef MP-WEIXIN */
.table-cell {
  font-size: 26rpx;
  padding: 20rpx 12rpx;
  min-height: 80rpx;
}

.header-cell {
  font-size: 28rpx;
}

.table-wrapper {
  border-radius: 8rpx;
}
/* #endif */

/* #ifdef MP-TOUTIAO */
.table-cell {
  font-size: 26rpx;
  padding: 20rpx 12rpx;
  min-height: 80rpx;
  line-height: 1.3;
}

.header-cell {
  font-size: 28rpx;
}

.table-wrapper {
  border-radius: 8rpx;
}

.table-container {
  padding: 16rpx;
}
/* #endif */

/* #ifdef MP-ALIPAY */
.table-cell {
  font-size: 26rpx;
  padding: 20rpx 12rpx;
}

.header-cell {
  font-size: 28rpx;
}
/* #endif */

/* #ifdef MP-BAIDU */
.table-cell {
  font-size: 26rpx;
  padding: 20rpx 12rpx;
}

.header-cell {
  font-size: 28rpx;
}
/* #endif */

/* #ifdef APP-PLUS */
.table-cell {
  font-size: 30rpx;
  padding: 26rpx 18rpx;
}

.header-cell {
  font-size: 32rpx;
}
/* #endif */

/* #ifdef APP-NVUE */
.table-cell {
  font-size: 30rpx;
}

.header-cell {
  font-size: 32rpx;
}
/* #endif */
</style>