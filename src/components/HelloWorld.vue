<template>
  <div class="relative range-wrap box border border-alpha-30 h-8 radius-4" :style="{width:width+'px'}">
    <div class="msk absolute top-0 left-0 w-100p h-100p radius-4" v-if="showMsk"></div>
    <div
        class="cover absolute top-0 left-0 w-100p h-100p radius-4"
        :style="style"
    ></div>
    <input
        class="absolute top-0 left-0"
        type="range"
        max="100"
        min="0"
        v-model="rangeNum"
        @change="changeEvent"
    />
  </div>
</template>

<script setup lang="ts">
import {
  defineEmits,
  defineProps,
  ref, watch
} from 'vue';

const emits = defineEmits(['update:num'])
const props = defineProps({
  width: {
    type: [String, Number],
    default: 140
  },
  num: {
    type: Number,
    default: 0
  },
  showMsk: {
    type: Boolean,
    default: false
  },
  style: {
    type: String,
    default: ''
  }
})
const rangeNum = ref(0);
watch(() => props.num, (val) => {
  rangeNum.value = val;
})
const changeEvent = () => {
  emits('update:num', rangeNum.value);
}
</script>

<style scoped lang="less">
input[type='range'] {
  -webkit-appearance: none;
}

input[type='range']::-webkit-slider-runnable-track {
  height: 6px;
  background: transparent;
  border: none;
  border-radius: 3px;
}

input[type='range']::-webkit-slider-thumb {
  -webkit-appearance: none;
  height: 10px;
  width: 10px;
  border-radius: 50%;
  background-color: transparent;
  border: 2px solid #fff;
  margin-top: -4px;
  box-shadow: 0px 0px 1px 0px rgba(0, 0, 0, 0.5);
}

input[type='range']:focus {
  outline: none;
}

input[type='range']:focus::-webkit-slider-runnable-track {
  background: transparent;
}

.msk {
  z-index: 1;
  background: #595959;
  background-image: linear-gradient(45deg,
  #ccc 25%,
  transparent 25%,
  transparent 75%,
  #ccc 75%,
  #ccc),
  linear-gradient(45deg,
  #ccc 26%,
  transparent 26%,
  transparent 74%,
  #ccc 74%,
  #ccc);
  background-size: 6px 6px;
  background-position: 0 0, 3px 3px;
}

.cover {
  z-index: 2;
}

.range-wrap {
  input[type='range'] {
    z-index: 3;
    width: 139px;
    left: -2px;
  }
}
</style>
<template>
  <!--  颜色选择区域-->
  <div class="pick-container" :style="{'background-color':rgbToHex(mainColor)}">
    <div class="white-cover"></div>
    <div class="black-cover"></div>
    <div>
      <div class="slide"
           :style="{'left': position.x + 'px','top': position.y + 'px'}"></div>
    </div>
    <div class="picker" @mousedown="mousedownEvent($event)"></div>
  </div>
  <!--  主色选择滑块-->
  <VRange v-model:num="colorRangeNum"
          style="background: -webkit-linear-gradient(left,red 0%,yellow 16.66%,lime 33.33%,aqua 50%,blue 66.66%,fuchsia 83.33%,red 100%);"></VRange>
  <!--  透明度选择滑块-->
  <VRange v-model:num="alphaRangeNum"
          :style="'background-image:linear-gradient(to right, transparent, ' + rgbToHex(pickColor) + ')'"
          :show-msk="true"></VRange>
  <div class="w-40 h-40" :style="{'background-color':rgbToHex(pickColor)}">
  </div>
  
<!--色值-->
    <div class="input-wrap">
      <div>
        <input type="text">
      </div>
    </div>
</template>

<script setup lang="ts">
import {
  ref,
  onMounted,
  onUnmounted,
  watch
} from 'vue';
import VRange from '@/components/public/VRange';
import {getColorBetween2Rgb, rgbToHex} from "@/utils/color";
// 色带
const colorList = [[255, 0, 0], [255, 255, 0], [0, 255, 0], [0, 255, 255], [0, 0, 255], [255, 0, 255], [255, 0, 0]];
const colorPercent = [0, 16.66, 33.33, 50, 66.66, 83.33, 100];
const width = 200, height = 100; // 颜色选择区域的宽高
const colorRangeNum = ref(0);
watch(colorRangeNum, (val) => {
  let index = 0;
  for (let i = colorPercent.length - 2; i >= 0; i--) {
    if (val >= colorPercent[i]) {
      index = i;
      break;
    }
  }
  mainColor.value = getColorBetween2Rgb(colorList[index], colorList[index+1], (val - colorPercent[index]) / 16.66 * 100);
})
const alphaRangeNum = ref(0);
// 颜色选择区域的主色
const mainColor = ref([255, 0, 0]);
// 选中的颜色
const pickColor = ref<number[]>([0, 0, 0]);
// 颜色选择区域滑块的位置
const position = ref({
  x: width / 2,
  y: height / 2
})
let isSliding = false;
const getPickColor = (x: number, y: number) => {
  const white = [255, 255, 255];
  const black = [0, 0, 0];
  // 获取x轴的颜色，即白色到目标色之间的颜色
  const xColor = getColorBetween2Rgb(white, mainColor.value, x / width * 100);
  // 获取最终颜色，及xColor到黑色之间的颜色
  pickColor.value = getColorBetween2Rgb(xColor, black, y / height * 100);
}
const pickEvent = (e: MouseEvent) => {
  const x = e.offsetX;
  const y = e.offsetY;
  if (x > width - 5 || y > height - 5 || x < 5 || y < 5) return;
  position.value = {
    x,
    y
  }
  getPickColor(x, y);
}
const mousedownEvent = (e: MouseEvent) => {
  e.stopPropagation();
  e.preventDefault();
  pickEvent(e)
  isSliding = true;
}

const mouseMoveEvent = (e: MouseEvent) => {
  if (isSliding) {
    pickEvent(e)
  }
}
const mouseUpEvent = () => {
  isSliding = false;
}
onMounted(() => {
  document.addEventListener('mousemove', mouseMoveEvent);
  document.addEventListener('mouseup', mouseUpEvent);
});
onUnmounted(() => {
  document.removeEventListener('mousemove', mouseMoveEvent);
  document.removeEventListener('mouseup', mouseUpEvent);
});
</script>

<style scoped lang="less">
.pick-container {
  width: 200px;
  height: 100px;
  position: relative;

  div {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
  }

  .white-cover {
    background: linear-gradient(90deg, #fff, rgba(255, 255, 255, 0));
  }

  .black-cover {
    background: linear-gradient(0deg, #000, transparent);
  }

  .slide {
    box-sizing: border-box;
    width: 10px;
    height: 10px;
    transform: translate(-5px, -5px);
    border: 1px solid #fff;
    box-shadow: 0px 0px 1px 0px rgba(0, 0, 0, 0.5000);
    position: absolute;
    border-radius: 50%;
    cursor: pointer;
  }

  .picker {
    cursor: pointer;
  }
}
</style>
export const hex3to6 = (hex: string) => {
  if (!hex.includes('#')) {
    hex = '#' + hex;
  }
  const argb = /^#?([a-f\d])([a-f\d])([a-f\d])$/i.exec(hex);
  if (argb && argb.length > 0) {
    return '#' + argb[1] + argb[1] + argb[2] + argb[2] + argb[3] + argb[3];
  }
  return hex;
}
// 将八位颜色转换为透明度+颜色
export const decodeColor = (val: string) => {
  const alpha = Math.round(parseInt(val.slice(1, 3), 16) / 255 * 100);
  const color = '#' + val?.slice(3)
  return {
    alpha,
    color
  }
}
// 将透明度+颜色合并成八位颜色
export const encodeColor = (param: {
  alpha: number,
  color: string
}) => {
  let alpha = Math.round(param.alpha / 100 * 255).toString(16);
  if (alpha.length < 2) {
    alpha = '0' + alpha;
  }
  return '#' + alpha + param.color.slice(1);
}
// rgb转hex
export const rgbToHex = (rgb: number[]) => {
  return rgb.reduce((a: string, b: number) => {
    let t = (b).toString(16);
    if (t.length < 2) {
      t = '0' + t;
    }
    return a + t;
  }, '#')
}
// hex转rgb
export const hexToRgb = (hex: string) => {
  hex = hex3to6(hex);
  const argb = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
  if (argb && argb.length > 0) {
    const r = parseInt(argb[1], 16);
    const g = parseInt(argb[2], 16);
    const b = parseInt(argb[3], 16);
    return [r, g, b];
  }
  return [255, 255, 255];
}

// 八位颜色转换为rgba
export const hex8toRgb = (val: string) => {
  const alphaStr = val.slice(1, 3);
  const alpha = parseInt(alphaStr, 16) / 255;
  const rgb = hexToRgb(val.slice(3));
  return 'rgba(' + rgb[0] + ',' + rgb[0] + ',' + rgb[0] + ',' + alpha.toFixed(2) + ')';
}

// 根据两个色值及位置百分比获取颜色
export const getColorBetween2Rgb = (r1: number[], r2: number[], p: number) :number[] => {
  let rgb = [];
  const r = (r2[0] - r1[0]) / 100;
  const g = (r2[1] - r1[1]) / 100;
  const b = (r2[2] - r1[2]) / 100;
  rgb = [
    Math.ceil(r1[0] + (p * r)),
    Math.ceil(r1[1] + (p * g)),
    Math.ceil(r1[2] + (p * b))
  ];
  return rgb;
}
