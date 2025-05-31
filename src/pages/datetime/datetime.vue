<template>
  <press-popup-plus
    :show="props.show"
    @update:show="(value) => emit('update:show', value)"
    position="bottom"
    round
    :custom-style="'height: 60%; z-index: 1199;'"
    @close="onClose"
  >
    <div class="picker-header">
      <div class="cancel-button" @click="onClose">取消</div>
      <div class="calendar-tabs">
        <div class="calendar-tab active">公历</div>
      </div>
      <div class="today-button" @click="handleToday">今</div>
    </div>
    
    <div class="picker-hint">
      <press-field
        v-model="inputDateTime"
        placeholder="输入出生年月日时分（格式199501010830）"
        class="date-input"
      />
      <press-button type="default" size="small" class="confirm-btn" @click="validateAndConfirm">确定</press-button>
    </div>
    
    <!-- Gregorian Calendar Picker -->
    <div class="picker-content">
      <press-datetime-picker 
        type="datetime"
        :value="gregorianDateValue"
        :min-date="minDate"
        :max-date="maxDate"
        :show-toolbar="false"
        @input="onGregorianDateChange"
      />
    </div>
    
    <!-- 底部确定按钮 -->
    <div class="bottom-confirm-button" @click="onConfirm">确定</div>
  </press-popup-plus>

  <press-toast id="press-toast" />
</template>

<script setup>
import PressToast from 'press-ui/press-toast/press-toast.vue';
import Toast from 'press-ui/press-toast';
import { ref, computed, watch } from 'vue';

import PressPopupPlus from 'press-ui/press-popup-plus/press-popup-plus.vue';
import PressButton from 'press-ui/press-button/press-button.vue';
import PressField from 'press-ui/press-field/press-field.vue';
import PressDatetimePicker from 'press-ui/press-datetime-picker/press-datetime-picker.vue';

const props = defineProps({
  show: Boolean,
  date: {
    type: Date,
    required: true
  }
});

const emit = defineEmits(['update:show', 'update:date', 'confirm']);

// 统一的主时间变量 - 使用公历时间戳作为唯一数据源
const selectedDateTime = ref(Date.now());

// 绑定默认值（注意顺序：年、月、日、时、分）
const inputDateTime = ref('');

// 为press-datetime-picker组件定义日期范围
const minDate = new Date(1901, 0, 1).getTime();
const maxDate = new Date(2099, 11, 31).getTime();

// 计算属性：直接使用统一的时间变量供press-datetime-picker使用
const gregorianDateValue = computed(() => {
  return selectedDateTime.value;
});

// Handle changes in the Gregorian picker (press-datetime-picker)
function onGregorianDateChange(timestamp) {
  console.log('Gregorian datetime picker changed:', timestamp);
  
  if (!timestamp) {
    console.warn('无效的时间戳', timestamp);
    return;
  }
  
  // 直接更新统一的时间变量
  selectedDateTime.value = timestamp;
}

// Handle today button
function handleToday() {
  const now = new Date();
  
  // 更新统一的时间变量
  selectedDateTime.value = now.getTime();
  
  // 更新日期值
  onConfirm();
}

// Handle close
function onClose() {
  emit('update:show', false);
}

// Handle confirmation with validation
function validateAndConfirm() {
  if (inputDateTime.value) {
    // 验证输入格式
    const datePattern = /^\d{12}$/;
    if (!datePattern.test(inputDateTime.value)) {
      // 使用Toast组件显示错误信息
      Toast('请输入12位数字，格式为年月日时分（如：199501010830）');
      return;
    }
    
    // 解析输入的日期字符串
    const year = parseInt(inputDateTime.value.substring(0, 4));
    const month = parseInt(inputDateTime.value.substring(4, 6));
    const day = parseInt(inputDateTime.value.substring(6, 8));
    const hour = parseInt(inputDateTime.value.substring(8, 10));
    const minute = parseInt(inputDateTime.value.substring(10, 12));
    
    // 验证日期的有效性
    if (month < 1 || month > 12 || day < 1 || day > 31 || hour < 0 || hour > 23 || minute < 0 || minute > 59) {
      Toast('请输入有效的日期时间');
      return;
    }
    
    // 更新统一的时间变量
    const newDate = new Date(year, month - 1, day, hour, minute);
    selectedDateTime.value = newDate.getTime();
  }
  
  onConfirm();
}

// Handle confirmation
function onConfirm() {
  // 使用统一的时间变量
  const newDate = new Date(selectedDateTime.value);
  
  // 更新统一的时间变量
  selectedDateTime.value = newDate.getTime();
  
  emit('confirm', newDate);
  emit('update:show', false);
}

// Initialize values when date changes
watch(() => props.date, (newDate) => {
  // 更新统一的时间变量
  // selectedDateTime.value = newDate.getTime();
}, { immediate: true });
</script>

<style scoped>
.picker-content {
  max-height: calc(100% - 200px);
  overflow-y: auto;
  padding-bottom: 10px;
}

.picker-header {
  display: flex;
  align-items: center;
  padding: 15px;
  border-bottom: 1px solid #f5f5f5;
}

.cancel-button, .today-button {
  color: #000;
  font-size: 16px;
  padding: 5px 10px;
  cursor: pointer;
}

.calendar-tabs {
  flex: 1;
  display: flex;
  justify-content: center;
  height: 40px;
  border-radius: 20px;
  background-color: #f5f5f5;
  padding: 5px;
  margin: 0 10px;
  overflow: hidden;
}

.calendar-tab {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  border-radius: 16px;
  cursor: pointer;
}

.calendar-tab.active {
  background-color: #000;
  color: #fff;
}

.picker-hint {
  padding: 10px 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #666;
  font-size: 14px;
}

.date-input {
  width: 85%;
}

.confirm-btn {
  margin-left: 10px;
  min-width: 50px;
}

.bottom-confirm-button {
  position: fixed;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  width: 90%;
  height: 50px;
  background-color: #000;
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  border-radius: 25px;
  cursor: pointer;
  z-index: 1299;
}
</style>