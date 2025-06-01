<template>
  <view class="content">
    <image class="logo" src="/static/logo.png" />
    <view class="text-area">
      <text class="title">{{ title }}</text>
    </view>
    <press-button
      type="default"
      :custom-style="customStyle"
      @click="onClick"
    >
      这是PressButton按钮1
    </press-button>
    <press-button
      type="primary"
      :custom-style="customStyle"
      @click="goToTabs"
    >
      查看tab下的数据表格
    </press-button>
    <press-button
      type="warning"
      @click="openArea"
    >
      打开地区选择弹窗
    </press-button>
    <press-button
      type="warning"
      @click="() => showArea2 = true"
    >
      打开地区选择弹窗2
    </press-button>
     <press-button
      type="warning"
      @click="() => showArea3 = true"
    >
      打开地区选择弹窗3
    </press-button>
    <press-button
      type="warning"
      @click="() => showDateTimePicker = true"
    >
      打开时间选择弹窗
    </press-button>

    <press-dialog id="press-dialog" />
    
    <AreaSelector 
      :show="showArea" 
      type="1"
      :date="currentDate"
      @update:show="(value) => showArea = value"
      @confirm="onAreaConfirm"
    />

    <AreaSelector 
      :show="showArea2" 
      type="2"
      :date="currentDate"
      @update:show="(value) => showArea2 = value"
      @confirm="onAreaConfirm"
    />
     <AreaSelector 
      :show="showArea3" 
      type="3"
      :date="currentDate"
      @update:show="(value) => showArea3 = value"
      @confirm="onAreaConfirm"
    />
    
    <datetime
      :show="showDateTimePicker"
      @update:show="(value) => showDateTimePicker = value"
    />
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

import PressButton from 'press-ui/press-button/press-button.vue'
import PressDialog from 'press-ui/press-dialog/press-dialog.vue'
import AreaSelector from '../area/area.vue'
import datetime from '../datetime/datetime.vue'
import { showConfirmDialog } from 'press-ui/press-dialog/handler';

const title = ref('Hello')
const showArea = ref(false)
const showArea2 = ref(false)
const showArea3 = ref(false)
const showDateTimePicker = ref(false)
const currentDate = ref(new Date())

const customStyle = 'margin-right: 50px;'

async function onClick(event) {
  console.log('click', event)
  showConfirmDialog({
    title: '标题',
    message: '弹窗内容',
  })
    .then(() => {
      // on confirm
      console.log('on confirm')
    })
    .catch(() => {
      // on cancel
      console.log('on cancel')
    });
}

function goToTabs() {
  uni.navigateTo({
    url: '/pages/tabs/tabs'
  })
}

function openArea() {
  showArea.value = true
}

function onAreaConfirm(locationData) {
  console.log('选择的地区:', locationData)
  showArea.value = false;
  showArea2.value = false;
  showArea3.value = false;
}

function showDateTime() {
  showDateTimePicker.value = true
}
</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin-top: 200rpx;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50rpx;
}

.text-area {
  display: flex;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}
</style>
