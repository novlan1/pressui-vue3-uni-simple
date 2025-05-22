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
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

import PressButton from 'press-ui/press-button/press-button.vue'
import { showConfirmDialog } from 'press-ui/press-dialog-plus/handler';

const title = ref('Hello')

const customStyle = 'margin-right: 50px;'

async function onClick(event) {
  console.log('click', event)
  try {
      console.log('showConfirmDialog111');
      await showConfirmDialog({
        title: '温馨提示',
        message: '游客每天免费3次，登录后获取更多额度',
        confirmButtonText: '去登录',
        cancelButtonText: '继续使用',
        showCancelButton: true,
        closeOnClickOverlay: true
      });
      console.log('showConfirmDialog222');
      return;
    } catch (e) {
      // 用户点击取消，检查使用次数
      console.log('showConfirmDialog333');
    }
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
