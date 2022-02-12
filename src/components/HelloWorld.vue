<!--
 * @Description: 
 * @version: 
 * @Author: jimmy
 * @Date: 2022-02-11 17:47:21
 * @LastEditors: jimmy
 * @LastEditTime: 2022-02-12 16:34:14
-->
<template>
  <div class="main_body">
    <div>
      长（厘米）：
      <input v-model="nowLength" />
    </div>
    <div>宽：(无需输入)</div>
    <div>
      高（厘米）：
      <input v-model="nowHeight" />
    </div>
    <div>
      重量（克）：
      <input v-model="nowWeight" />
    </div>
    <button @click="calLens(nowLength)">计算</button>
  </div>
  <div class="main_body result">
    <div style="display: flex; justify-content: center;" v-if="mailExpress[0]">
      <div v-for="(item,index) of mailExpress[0].range" :key="item" style="margin: 0 10px;">
        <div>{{mailCountry[index]}}</div>
        <div>{{ item }}</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
const nowWeight = ref(0);
const nowHeight = ref(0);
const nowLength = ref(0);
const mailExpress = ref([]);
const mailCountry = ref(['英国', '德国3', '法国', 'IT', '西班牙', '荷兰', '瑞典', '波兰'])
const rangeMap = {
  a: [
    { min: 0, max: 80, range: ['£1.54', '€ 1.80', '€ 2.52', '€ 2.88', '€ 2.36', '€ 1.67', '26.05', '4.48'] }
  ],
  b: [
    { min: 0, max: 60, range: ['£1.64', '€ 1.98', '€ 2.62', '€ 3.00', '€ 2.65', '€ 1.81', '26.76', '4.53'] },
    { min: 60, max: 210, range: ['£1.79', '€ 2.12', '€ 3.13', '€ 3.13', '€ 2.96', '€ 1.99', '27.92', '4.66'] },
    { min: 210, max: 460, range: ['£1.91', '€ 2.27', '€ 3.58', '€ 3.34', '€ 3.18', '€ 2.11', '32.16', '4.91'] }
  ],
  c: [
    { min: 0, max: 960, range: ['£2.33', '€ 2.60', '€ 4.16', '€ 3.61', '€ 3.33', '€ 2.55', '33.43', '5.64'] }
  ],
  d: [
    { min: 0, max: 960, range: ['£2.52', '€ 2.96', '€ 4.48', '€ 3.83', '€ 3.54', '€ 2.80', '36.63', '5.7'] }
  ],
  e: [
    { min: 0, max: 150, range: ['£2.37', '€ 2.87', '€ 4.19', '€ 3.79', '€ 3.23', '€ 2.61', '37.51', '5.35'] },
    { min: 150, max: 400, range: ['£2.52', '€ 3.18', '€ 4.97', '€ 4.22', '€ 3.77', '€ 2.89', '39.3', '5.66'] },
    { min: 400, max: 900, range: ['£2.70', '€ 3.55', '€ 5.68', '€ 4.72', '€ 3.99', '€ 3.36', '39.84', '6.48'] },
    { min: 900, max: 1400, range: ['£2.87', '€ 4.19', '€ 5.91', '€ 5.30', '€ 4.44', '€ 3.95', '41.03', '6.75'] },
    { min: 1400, max: 1900, range: ['£3.14', '€ 4.56', '€ 5.98', '€ 5.54', '€ 4.51', '€ 4.24', '42.41', '6.75'] },
    { min: 1900, max: 3900, range: ['£5.03', '€ 5.72', '€ 9.16', '€ 7.31', '€ 5.87', '€ 5.49', '50.5', '6.86'] },
  ],
  f: [
    { min: 0, max: 150, range: ['£2.57', '€ 3.09', '€ 4.46', '€ 4.28', '€ 3.49', '€ 2.81', '40.45', '5.41'] },
    { min: 150, max: 400, range: ['£2.77', '€ 3.48', '€ 5.27', '€ 4.82', '€ 4.10', '€ 3.16', '42.91', '5.71'] },
    { min: 400, max: 900, range: ['£2.99', '€ 3.94', '€ 6.14', '€ 5.48', '€ 4.42', '€ 3.63', '43.01', '6.55'] },
    { min: 900, max: 1400, range: ['£3.19', '€ 4.64', '€ 6.49', '€ 6.19', '€ 5.09', '€ 4.33', '44.96', '6.81'] },
    { min: 1400, max: 1900, range: ['£3.49', '€ 5.10', '€ 6.68', '€ 6.43', '€ 5.18', '€ 4.74', '47.45', '6.82'] },
    { min: 1900, max: 2900, range: ['£4.95', '€ 5.70', '€ 8.95', '€ 7.28', '€ 5.89', '€ 5.50', '50.63', '6.88'] },
    { min: 2900, max: 3900, range: ['£5.39', '€ 6.28', '€ 9.34', '€ 7.60', '€ 7.20', '€ 5.52', '50.78', '6.93'] },
    { min: 3900, max: 5900, range: ['£5.54', '€ 6.61', '€ 9.80', '€ 8.68', '€ 7.44', '€ 5.74', '54.49', '7.01'] },
    { min: 5900, max: 8900, range: ['£6.32', '€ 7.13', '€ 10.66', '€ 9.62', '€ 7.46', '€ 6.05', '55.98', '7.39'] },
    { min: 8900, max: 11900, range: ['£6.68', '€ 7.41', '€ 11.17', '€ 10.32', '€ 7.47', '€ 6.45', '73.28', '9.12'] },
  ],
  g: [
    { min: 0, max: 760, range: ['£4.33', '€ 5.89', '€ 8.50', '€ 8.05', '€ 6.27', '€ 5.81', '65.79', '7'] },
    { min: 760, max: 1260, range: ['£5.02', '€ 5.91', '€ 8.86', '€ 8.48', '€ 6.87', '€ 5.96', '67.46', '7.21'] },
    { min: 1260, max: 1760, range: ['£5.18', '€ 5.93', '€ 9.44', '€ 8.59', '€ 6.96', '€ 6.02', '68.14', '7.31'] },
    { min: 1760, max: 999999, range: ['£0.01每kg（超过1.76kg）', '€0.01每kg（超过1.76kg）', '€0.01每kg（超过1.76kg）', '€0.01每kg（超过1.76kg）', '€0.01每kg（超过1.76kg）', '€0.01每kg（超过1.76kg）', '0.10每kg（超过1.76kg）', '0.05每kg（超过1.76kg）'] },
  ],
  h: [
    { min: 0, max: 760, range: ['£5.15', '€ 5.95', '€ 8.51', '€ 8.53', '€ 6.31', '€ 5.86', '66.41', '7'] },
    { min: 760, max: 1760, range: ['£5.44', '€ 6.24', '€ 9.61', '€ 8.67', '€ 6.99', '€ 6.12', '69.32', '7.31'] },
    { min: 1760, max: 2760, range: ['£5.56', '€ 7.00', '€ 10.09', '€ 8.68', '€ 7.66', '€ 7.15', '80.93', '8.53'] },
    { min: 2760, max: 3760, range: ['£5.59', '€ 7.06', '€ 10.51', '€ 9.27', '€ 7.72', '€ 7.21', '81.59', '8.6'] },
    { min: 3760, max: 4760, range: ['£5.62', '€ 7.08', '€ 10.58', '€ 9.31', '€ 7.97', '€ 7.24', '81.59', '8.6'] },
    { min: 4760, max: 9760, range: ['£6.72', '€ 7.44', '€ 11.40', '€ 10.56', '€ 11.66', '€ 7.53', '85.29', '9.12'] },
    { min: 9760, max: 14760, range: ['£7.19', '€ 8.10', '€ 12.23', '€ 11.73', '€ 12.59', '€ 8.28', '93.74', '9.88'] },
    { min: 14760, max: 19760, range: ['£7.54', '€ 8.61', '€ 12.85', '€ 12.09', '€ 13.65', '€ 8.38', '99.68', '10.51'] },
    { min: 19760, max: 24760, range: ['£8.35', '€ 9.75', '€ 12.85', '€ 12.87', '€ 13.66', '€ 8.45', '111.78', '11.9'] },
    { min: 24760, max: 29760, range: ['£8.36', '€ 9.76', '€ 14.31', '€ 13.51', '€ 15.17', '€ 8.46', '111.83', '11.9'] },
    { min: 29760, max: 999999, range: ['£0.01每kg（超过29.76kg）', '€0.01每kg（超过29.76kg）', '€0.01每kg（超过29.76kg）', '€0.01每kg（超过29.76kg）', '€0.01每kg（超过29.76kg）', '€0.01每kg（超过29.76kg）', '0.10每kg（超过29.76kg）', '0.05每kg（超过29.76kg）'] },
  ],
  i: [
    { min: 0, max: 4760, range: ['£9.34', '€ 8.53', '€ 15.59', '€ 9.44', '€ 10.03', '€ 8.33', '94.33', '9.34'] },
    { min: 4760, max: 9760, range: ['£10.21', '€ 9.83', '€ 18.99', '€ 10.75', '€ 13.45', '€ 9.61', '108.8', '10.77'] },
    { min: 9760, max: 14760, range: ['£10.79', '€ 10.14', '€ 20.00', '€ 11.83', '€ 14.53', '€ 10.28', '116.4', '11.52'] },
    { min: 14760, max: 19760, range: ['£11.30', '€ 10.72', '€ 20.98', '€ 12.21', '€ 15.70', '€ 10.86', '123', '12.18'] },
    { min: 19760, max: 24760, range: ['£12.30', '€ 11.86', '€ 22.93', '€ 13.69', '€ 17.02', '€ 12.03', '136.22', '13.49'] },
    { min: 24760, max: 31500, range: (8)['£12.33', '€ 11.89', '€ 23.46', '€ 13.77', '€ 19.51', '€ 12.05', '136.5', '13.51'] },
    { min: 31500, max: 999999, range: ['£0.01每kg（超过31.50kg）', '€0.01每kg（超过31.50kg）', '€0.01每kg（超过31.50kg）', '€0.01每kg（超过31.50kg）', '€0.01每kg（超过31.50kg）', '€0.01每kg（超过31.50kg）', '0.10每kg（超过31.50kg）', '0.05每kg（超过31.50kg）'] },
  ]
}

const calWeight = (weight, range) => {
  console.log('weight', weight)
  console.log('range', range)
  if (range.length == 1) {
    mailExpress.value = range;
    console.log('mailExpress1', mailExpress.value)
    return;
  }
  const res = range.filter(item => {
    console.log('item', item)
    return weight <= item.max && weight > item.min;
  })
  console.log('res123', res)
  mailExpress.value = res;
  console.log('mailExpress2', mailExpress.value)
}

const calcHeight = (height) => {
  switch (true) {
    case height <= 1:
      calWeight(nowWeight.value, rangeMap.a)
      break;
    case height <= 2.5 && height > 1:
      calWeight(nowWeight.value, rangeMap.b)
      break;
    case height <= 4 && height > 2.5:
      calWeight(nowWeight.value, rangeMap.c)
      break;
    case height <= 6 && height > 4:
      calWeight(nowWeight.value, rangeMap.d)
      break;
    default:
      break;
  }
}

const calLens = (len) => {
  console.log('len', len)
  switch (true) {
    case len <= 20:
      calWeight(nowWeight.value, rangeMap.a);
      break;
    case len > 20 && len <= 33:
      calcHeight(nowHeight.value)
      break;
    case len > 33 && len <= 35:
      calWeight(nowWeight.value, rangeMap.e);
      break;
    case len > 35 && len <= 45:
      calWeight(nowWeight.value, rangeMap.f);
      break;
    case len > 45 && len <= 61:
      calWeight(nowWeight.value, rangeMap.g);
      break;
    case len > 61 && len <= 120:
      calWeight(nowWeight.value, rangeMap.h);
      break;
    case len > 120 && len <= 150:
      calWeight(nowWeight.value, rangeMap.i);
      break;
    case len > 175:
      break;
    default:
      console.log('defalut')
      break;
  }
}


</script>

<style scoped>
.main_body {
  display: flex;
  align-items: center;
  justify-content: center;
}
.main_body > div {
  margin: 0 20px;
}

.result {
  margin-top: 20px;
  font-size: 24px;
  font-weight: bold;
  display: flex;
  flex-direction: column;
}
</style>
