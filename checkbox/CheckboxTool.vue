<script setup lang="ts">
import { reactive, watch } from 'vue'

// 默认配置
const defaultConfig = {
  list: [],
  text: 'text',
  desc: 'desc',
  single: false,
}

// 默认样式
const defaultPattern = {
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
 * @brief: checkbox 的 value 变更时
 * @param {*} index
 * @return {*}
 ********************************************************************************/
const checkValueChange = (index: number) => {
  if (
    check(compProps.single, compConfig.single) &&
    compConfig.list[index].value
  ) {
    compConfig.list.forEach((o) => (o.value = false))
    compConfig.list[index].value = true
  }
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
</script>

<template>
  <div
    class="checkbox-tool-content"
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
      class="checkbox-desc"
      :style="{
        lineHeight: compPattern.descLine,
        marginBottom: compPattern.descMargin,
        fontSize: compPattern.descFontSize,
      }"
    >
      {{ check(compProps.desc, compConfig.desc) }}
    </div>
    <div
      class="checkbox-box"
      :style="{ marginBottom: compPattern.textMargin, gap: compPattern.boxGap }"
    >
      <div
        class="checkbox-wrapper"
        v-for="(v, k) in check(compProps.list, compConfig.list)"
      >
        <input
          checked=""
          type="checkbox"
          class="check"
          v-model="compConfig.list[k].value"
          :id="`${compConfig.list[k].item}`"
          @change="checkValueChange(k)"
        />
        <label
          :for="`${compConfig.list[k].item}`"
          class="label"
        >
          <svg
            width="45"
            height="45"
            viewBox="0 0 95 95"
          >
            <rect
              x="30"
              y="20"
              width="50"
              height="50"
              stroke="black"
              fill="none"
            ></rect>
            <g transform="translate(0,-952.36222)">
              <path
                d="m 56,963 c -102,122 6,9 7,9 17,-5 -66,69 -38,52 122,-77 -7,14 18,4 29,-11 45,-43 23,-4"
                stroke="black"
                stroke-width="3"
                fill="none"
                class="path1"
              ></path>
            </g>
          </svg>
          <span>{{ compConfig.list[k].item }}</span>
        </label>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.checkbox-tool-content {
  @include global.wh(100%, auto);
  @include global.flex_config(1, space-between);
  .first-line {
    @include global.wh(100%, 50px);
    @include global.flex_config(0, space-between);
    @include global.font_config(1.5vw, rgba(51, 51, 51, 1));
    // margin-bottom: 12px;
  }
  .checkbox-desc {
    @include global.wh(100%, 50px);
    text-align: start;
    word-break: break-all;
    @include global.font_config(1.2vw, rgba(51, 51, 51, 0.8));
  }
  // margin-bottom: 12px;
  .checkbox-box {
    @include global.wh(100%, auto);
    display: flex;
    justify-content: flex-start;
    flex-flow: row wrap;
    gap: 20px;
  }
}
.checkbox-wrapper input[type='checkbox'] {
  visibility: hidden;
  display: none;
}

.checkbox-wrapper *,
.checkbox-wrapper ::after,
.checkbox-wrapper ::before {
  box-sizing: border-box;
  user-select: none;
}

.checkbox-wrapper {
  position: relative;
  display: block;
  overflow: hidden;
  // margin-right: 12px;
  // flex: 1;
  flex-grow: 1;
}

.checkbox-wrapper .label {
  cursor: pointer;
}

.checkbox-wrapper .check {
  width: 50px;
  height: 50px;
  position: absolute;
  opacity: 0;
}

.checkbox-wrapper .label svg {
  vertical-align: middle;
}

.checkbox-wrapper .path1 {
  stroke-dasharray: 400;
  stroke-dashoffset: 400;
  transition: 0.5s stroke-dashoffset;
  opacity: 0;
}

.checkbox-wrapper .check:checked + label svg g path {
  stroke-dashoffset: 0;
  opacity: 1;
}
</style>
