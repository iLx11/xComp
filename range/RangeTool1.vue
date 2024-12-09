<script setup lang="ts">
import { onMounted, reactive, watch, computed } from 'vue'

// 默认配置
const defaultConfig = {
  value: 0,
  max: 100,
  min: 0,
  step: 1,
  text: 'text',
  desc: 'desc',
}

// 默认样式
const defaultPattern = {
  moveLeft: '0%',
  textMargin: '20px',
  boxMargin: '20px',
  textFontSize: '1.5vw',
  boxPadding: '0',
  boxGap: '10px',
  textAlign: 'start',
  descLine: 1.2,
  descMargin: '20px',
  descFontSize: '1.2vw',
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
const compConfig = reactive({ ...defaultConfig, ...props.config })
const compProps = reactive(props.config)

// 留下更新接口
watch(
  compConfig,
  () => {
    emit('update:config', { ...compConfig })
  },
  { deep: true }
)

/********************************************************************************
 * @brief: 样式副本
 ********************************************************************************/
const compPattern = reactive({ ...defaultPattern, ...props.pattern })

/********************************************************************************
 * @brief: range 更改回调
 * @return {*}
 ********************************************************************************/
const rangeChange = () => {
  let leftValue = Math.ceil(
    (compConfig.value /
      (check(compProps.max, compConfig.max) -
        check(compProps.min, compConfig.min))) *
      100
  )
  console.info(compConfig.value, compConfig.max)

  compPattern.moveLeft = leftValue * -2 + '%'
  console.info(compPattern.moveLeft)
}

/********************************************************************************
 * @brief: 返回数据状态
 * @param {*} before
 * @param {*} after
 * @return {*}
 ********************************************************************************/
const check = (before, after) => {
  return before ? before : after
}

onMounted(() => {
  generateTicks()
})

const ticks = reactive([])

const generateTicks = () => {
  if (compConfig.min >= compConfig.max) {
    return
  }
  const step = (compConfig.max - compConfig.min) / 20
  for (let i = 0; i <= 20; i++) {
    ticks.push(compConfig.min + i * step)
  }
  console.info(ticks)
}
</script>

<template>
  <div
    class="range-tool-content"
    :style="{
      marginBottom: compPattern.boxMargin,
      padding: compPattern.boxPadding,
    }"
  >
    <div
      class="first-line"
      :style="{
        marginBottom: compPattern.textMargin,
        textAlign: compPattern.textAlign,
      }"
    >
      <div
        class="checkbox-text"
        :style="{ fontSize: compPattern.textFontSize }"
      >
        {{ check(compProps.text, compConfig.text) }}
      </div>
    </div>
    <div
      class="range-desc"
      :style="{
        lineHeight: compPattern.descLine,
        marginBottom: compPattern.descMargin,
        fontSize: compPattern.descFontSize,
      }"
    >
      {{ check(compProps.desc, compConfig.desc) }}
    </div>
    <div
      class="range-box"
      :style="{ marginBottom: compPattern.textMargin, gap: compPattern.boxGap }"
    >
      <input
        class="range-input"
        type="range"
        @input="rangeChange"
        v-model="compConfig.value"
        :min="check(compProps.min, compConfig.min)"
        :max="check(compProps.max, compConfig.max)"
        :step="check(compProps.step, compConfig.step)"
      />
      <div class="range-text-box">
        <div class="range-center-line"></div>
        <div
          class="range-move-box"
          :style="{ left: compPattern.moveLeft }"
        >
          <div class="tick-short-line"></div>

          <div
            v-for="tick in ticks"
            :key="tick"
            class="tick-box"
          >
            <div>{{ tick }}</div>
            <div class="tick-line"></div>
          </div>
        </div>
        <!-- <div
          class="range-text"
          :style="{ left: compPattern.moveLeft }"
        >
          {{ compConfig.value }}
        </div> -->
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.range-tool-content {
  @include global.wh(100%, auto);
  @include global.flex_config(1, space-between);
  // margin-bottom: 20px;
  .first-line {
    @include global.wh(100%, 50px);
    @include global.flex_config(0, space-between);
    @include global.font_config(1.5vw, rgba(51, 51, 51, 1));
    // margin-bottom: 12px;
  }
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

// Range Slider
$range-handle-color: $shade-10 !default;
$range-handle-color-hover: $teal !default;
$range-handle-size: 20px !default;
$range-track-color: $shade-1 !default;

.range-box {
  @include global.wh(100%, 80px);
  padding: 0 20px;
  @include global.flex_config(1, space-between);
  // margin-bottom: 20px;
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
  @include global.wh(102%, 50px);
  position: relative;
  overflow: scroll;
  mask: linear-gradient(90deg, #000 70%, transparent);

  .range-center-line {
    @include global.wh(7px, 20px);
    background: rgb(249, 141, 141);
    @include global.pos_ab(-5%, 50%);
    transform: translate(-50%, 0);
    z-index: 99;
  }

  .tick-short-line {
    @include global.wh(100%, 20px);
    @include global.pos_ab;
    background: repeating-linear-gradient(
      90deg,
      rgba(51, 51, 51, 0.2),
      rgba(51, 51, 51, 0.2) 0.1%,
      transparent 0.1%,
      transparent 1%
    );
  }

  .range-move-box {
    @include global.wh(200%, 100%);
    @include global.pos_ab;
    background: rgba(172, 186, 189, 0.3);
    @include global.flex_config(0, space-between);
    transform: translate(25%);
    .tick-box {
      @include global.wh(1px, 100%);
      text-align: center;
      height: 100%;
      position: relative;
      @include global.flex_config(1, flex-end);
    }
    .tick-line {
      @include global.wh(3px, 25px);
      background: rgba(51, 51, 51, 0.5);
      @include global.pos_ab(0, 50%);
      transform: translate(-50%);
    }
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
