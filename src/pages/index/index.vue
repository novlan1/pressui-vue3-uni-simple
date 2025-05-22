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
      @click="onClick"
    >
      这是PressButton按钮2
    </press-button>
    <press-dialog-plus id="press-dialog" />
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

import PressButton from 'press-ui/press-button/press-button.vue'
import PressDialogPlus from 'press-ui/press-dialog-plus/press-dialog-plus.vue'
import { showConfirmDialog } from 'press-ui/press-dialog-plus/handler';

const title = ref('Hello')

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
