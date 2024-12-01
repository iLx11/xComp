<script setup lang="ts">
import { onMounted, reactive, watch } from 'vue'

// 默认配置
const defaultConfig = {
  rangeValue: 0,
  rangeMax: 100,
  rangeMin: 0,
  rangeStep: 10,
  rangeDesc: 'desc text',
}

// 默认样式
const defaultPattern = {
  rangeTextLeft: '0%',
}

const props = defineProps({
  config: {
    type: Object,
    default: () => ({}),
  },
  pattern: {
    type: Object,
    default: () => ({}),
  },
})

const emit = defineEmits(['update:config'])

/********************************************************************************
 * @brief: 配置副本
 ********************************************************************************/
const rangeConfig = reactive({ ...defaultConfig, ...props.config })

watch(
  rangeConfig,
  () => {
    emit('update:config', { ...rangeConfig })
  },
  { deep: true }
)

/********************************************************************************
 * @brief: 样式副本
 ********************************************************************************/
const switchPattern = reactive({ ...defaultPattern, ...props.pattern })

/********************************************************************************
 * @brief: range 更改回调
 * @return {*}
 ********************************************************************************/
const rangeChange = () => {
  let leftValue =
    (rangeConfig.rangeValue / (rangeConfig.rangeMax - rangeConfig.rangeMin)) *
    100
  switchPattern.rangeTextLeft = leftValue + '%'
}
</script>

<template>
  <div class="range-tool-content">
    <div class="range-desc">{{ rangeConfig.rangeDesc }}</div>
    <div class="range-box">
      <input
        class="range-input"
        type="range"
        value="0"
        v-model="rangeConfig.rangeValue"
        :min="rangeConfig.rangeMin"
        :max="rangeConfig.rangeMax"
        :step="rangeConfig.rangeStep"
        @input="rangeChange"
      />
      <div class="range-text-box">
        <div
          class="range-text"
          :style="{ left: switchPattern.rangeTextLeft }"
        >
          {{ rangeConfig.rangeValue }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.range-tool-content {
  @include global.wh(100%, auto);
  @include global.flex_config(1, space-between);
  margin-bottom: 20px;
}
// Base Colors
$shade-10: #2c3e50 !default;
$shade-1: #d7dcdf !default;
$shade-0: #fff !default;
$teal: #e1a08e !default;

.range-desc {
  @include global.wh(100%, auto);
  margin-bottom: 25px;
  text-align: start;
  word-break: break-all;
}

.range-box {
  @include global.wh(100%, 90px);
  padding: 0 20px;
  // @include global.flex_ce;
}

// Range Slider
$range-width: 100% !default;

$range-handle-color: $shade-10 !default;
$range-handle-color-hover: $teal !default;
$range-handle-size: 20px !default;

$range-track-color: $shade-1 !default;
$range-track-height: 10px !default;

$range-label-color: $shade-10 !default;
$range-label-width: 60px !default;

.range-box {
  width: $range-width;
  @include global.flex_config(1, space-between);
}

.range-input {
  -webkit-appearance: none;
  @include global.wh(100%, 15px);
  border-radius: 8px;
  background: $range-track-color;
  outline: none;
  padding: 0;
  margin: 0;

  // Range Handle
  &::-webkit-slider-thumb {
    appearance: none;
    width: $range-handle-size;
    height: $range-handle-size;
    border-radius: 50%;
    background: $range-handle-color;
    cursor: pointer;
    transition: background 0.15s ease-in-out;

    &:hover {
      background: $range-handle-color-hover;
    }
  }

  &:active::-webkit-slider-thumb {
    background: $range-handle-color-hover;
  }

  &::-moz-range-thumb {
    @include global.wh($range-handle-size, $range-handle-size);
    border: 0;
    border-radius: 50%;
    background: $range-handle-color;
    cursor: pointer;
    transition: background 0.15s ease-in-out;

    &:hover {
      background: $range-handle-color-hover;
    }
  }

  &:active::-moz-range-thumb {
    background: $range-handle-color-hover;
  }

  // Focus state
  &:focus {
    &::-webkit-slider-thumb {
      box-shadow: 0 0 0 3px $shade-0, 0 0 0 6px $teal;
    }
  }
}

.range-text-box {
  @include global.wh(calc(100% - 20px), 40px);
  position: relative;
}

// Range Label
.range-text {
  transition: left 0.1s ease-in-out;
  @include global.wh(45px, 45px);
  @include global.pos_ab(0, 0, 3);
  @include global.style_common(12px, $range-handle-color);
  transform: translateX(-50%);
  @include global.flex_center;
  @include global.font_config(1.2vw, rgba(255, 255, 255, 1));
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 50%;
    transform: translate(-50%, -60%);
    width: 100%;
    height: 50%;
    background: $range-handle-color;
    clip-path: polygon(0 100%, 50% 0, 100% 100%);
  }
}

// Firefox Overrides
::-moz-range-track {
  background: $range-track-color;
  border: 0;
}

input::-moz-focus-inner,
input::-moz-focus-outer {
  border: 0;
}
</style>
