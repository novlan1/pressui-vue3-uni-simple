<template>
  <press-popup-plus
    :show="props.show"
    @update:show="(value) => emit('update:show', value)"
    position="bottom"
    round
    :custom-style="'height: 60%; z-index: 1199;'"
    @close="onClose"
  >
    <div class="location-header">
      <div class="cancel-button" @click="onClose">取消</div>
      <div class="location-title">选择地区</div>
    </div>

    <div class="location-headers">
      <div class="location-header-item">省份</div>
      <div class="location-header-item">城市</div>
      <div class="location-header-item">区县</div>
    </div>

    <div class="location-content">
      <press-area
        ref="pressArea"
        :area-list="computedAreaList"
        :value="selectedAreaCode"
        title="选择地区"
        :visible-item-count="6"
        :item-height="40"
        :show-toolbar="false"
        @change="onAreaChange"
        @confirm="onAreaConfirm"
        @cancel="onClose"
      />
    </div>
    
    <div class="bottom-confirm-button" @click="onConfirm">确定</div>
  </press-popup-plus>
</template>

<script setup>
import { ref, reactive, computed } from 'vue';

import PressPopupPlus from 'press-ui/press-popup-plus/press-popup-plus.vue';
import PressArea from 'press-ui/press-area/press-area.vue';

// 简化的地区数据
const areaList = {
  province_list: {
    '110000': '北京市',
    '120000': '天津市',
    '130000': '河北省',
    '140000': '山西省',
    '310000': '上海市',
    '320000': '江苏省',
    '330000': '浙江省',
    '440000': '广东省'
  },
  city_list: {
    '110100': '北京市',
    '120100': '天津市',
    '130100': '石家庄市',
    '130200': '唐山市',
    '140100': '太原市',
    '310100': '上海市',
    '320100': '南京市',
    '320200': '无锡市',
    '330100': '杭州市',
    '330200': '宁波市',
    '440100': '广州市',
    '440300': '深圳市'
  },
  county_list: {
    '110101': '东城区',
    '110102': '西城区',
    '110105': '朝阳区',
    '110106': '丰台区',
    '120101': '和平区',
    '120102': '河东区',
    '130102': '长安区',
    '130104': '桥西区',
    '140105': '小店区',
    '140106': '迎泽区',
    '310101': '黄浦区',
    '310104': '徐汇区',
    '320102': '玄武区',
    '320104': '秦淮区',
    '330102': '上城区',
    '330103': '下城区',
    '440103': '荔湾区',
    '440104': '越秀区',
    '440303': '罗湖区',
    '440304': '福田区'
  }
};


const props = defineProps({
  show: Boolean,
  type: {
    type: String,
    default: '1'
  },
  date: {
    type: Date,
    default: () => new Date()
  }
});

const computedAreaList  = computed(() => {
  return {
    ...areaList,
    province_list:{
      [props.type]: props.type,
      ...areaList.province_list,
    }
  }
})

const emit = defineEmits(['update:show', 'confirm']);

// 选中的地区信息
const selectedLocation = reactive({
  province: null,
  city: null,
  district: null,
  fullName: ''
});

// 默认选中北京市
const selectedAreaCode = ref('110100');

// 地区选择变化事件
function onAreaChange(event) {
  console.log('Area change:', event);
  const { values } = event;
  if (values && values.length > 0) {
    updateLocationFromValues(values);
  }
}

// 地区确认事件
function onAreaConfirm(event) {
  console.log('Area confirm:', event);
  const { values } = event;
  if (values && values.length > 0) {
    updateLocationFromValues(values);
  }
  onConfirm();
}

// 根据选择的值更新位置信息
function updateLocationFromValues(values) {
  if (values.length >= 1) {
    selectedLocation.province = values[0];
  }
  if (values.length >= 2) {
    selectedLocation.city = values[1];
  }
  if (values.length >= 3) {
    selectedLocation.district = values[2];
  }
  
  updateFullName();
}

// 更新完整地名
function updateFullName() {
  const parts = [];
  if (selectedLocation.province) parts.push(selectedLocation.province.name);
  if (selectedLocation.city) parts.push(selectedLocation.city.name);
  if (selectedLocation.district) parts.push(selectedLocation.district.name);
  
  selectedLocation.fullName = parts.join('');
}

// 关闭弹窗
function onClose() {
  emit('update:show', false);
}

// 确认选择
function onConfirm() {
  const locationData = {
    name: selectedLocation.fullName || '北京市',
    province: selectedLocation.province,
    city: selectedLocation.city,
    district: selectedLocation.district
  };
  
  emit('confirm', locationData);
  emit('update:show', false);
}
</script>

<style scoped>
.location-header {
  display: flex;
  align-items: center;
  padding: 15px;
  border-bottom: 1px solid #f5f5f5;
}

.cancel-button {
  color: #000;
  font-size: 16px;
  padding: 5px 10px;
  cursor: pointer;
  width: 50px;
}

.location-title {
  flex: 1;
  text-align: center;
  font-size: 16px;
  font-weight: 500;
}

.location-headers {
  display: flex;
  padding: 5px;
  border-bottom: 1px solid #e5e5e5;
}

.location-header-item {
  flex: 1;
  text-align: center;
  font-size: 15px;
  font-weight: 500;
}

.location-content {
  flex: 1;
  overflow-y: auto;
  height: calc(60%);
  position: relative;
}

.bottom-confirm-button {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 44px;
  background-color: #000;
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  cursor: pointer;
  border-radius: 22px;
  margin: 10px 16px;
  z-index: 1299;
}
</style>