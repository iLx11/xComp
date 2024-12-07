<script setup lang="ts">
import { reactive, watch } from 'vue'

// 默认配置
const defaultConfig = {
  list: [],
  text: 'text',
  desc: 'desc',
  single: false,
  writable: false,
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
  if (compConfig.single && !compConfig.list[index].value) {
    compConfig.list.forEach((o) => (o.value = false))
  }
  compConfig.list[index].value = !compConfig.list[index].value
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
        v-for="(v, k) in check(compProps.list, compConfig.list)"
        :key="v"
        @click="checkValueChange(k)"
        :class="{ 'checkbox-selected': v.value }"
      >
        <div v-if="!check(compProps.writable, compConfig.writable)">
          {{ v.item }}
        </div>
        <input
          type="text"
          v-model="compConfig.list[k].item"
          v-else
        />
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.checkbox-selected {
  background: rgb(176, 195, 209) !important;
  color: rgba(255, 255, 255, 0.8);
}
.checkbox-tool-content {
  @include global.wh(100%, auto);
  @include global.flex_config(1, space-between);
  .first-line {
    @include global.wh(100%, auto);
    @include global.flex_config(0, space-between);
    @include global.font_config(1.5vw, rgba(51, 51, 51, 1));
    // margin-bottom: 12px;
  }
  .checkbox-text {
    @include global.wh(100%, auto);
    word-break: break-all;
  }
  .checkbox-desc {
    @include global.wh(100%, auto);
    text-align: start;
    word-break: break-all;
    @include global.font_config(1.2vw, rgba(51, 51, 51, 0.8));
  }
  // margin-bottom: 12px;
  .checkbox-box {
    @include global.wh(100%, auto);
    @include global.flex_config(0, space-between);
    // gap: 10px;

    > div {
      @include global.wh(auto, 70px);
      @include global.flex_center;
      flex: 1;
      border-radius: 10px;
      font-size: 1vw;
      background: rgba(51, 51, 51, 0.1);
      @include global.font_config(1.2vw, rgba(51, 51, 51, 0.8));
      font-weight: 800;
      cursor: pointer;
      input {
        @include global.full_wh;
        outline: none;
        border: none;
        background-color: rgba(255, 255, 255, 0);
        text-align: center;
        @include global.font_config(1.2vw, rgba(51, 51, 51, 0.8));
      }
    }
  }
}
</style>
