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
        marginBottom: compPattern.descMargin,
        lineHeight: compPattern.descLine,
        fontSize: compPattern.descFontSize,
      }"
    >
      {{ check(compProps.desc, compConfig.desc) }}
    </div>
    <div
      class="checkbox-box"
      :style="{ marginBottom: compPattern.textMargin, gap: compPattern.boxGap }"
    >
      <div v-for="(v, k) in check(compProps.list, compConfig.list)">
        <label class="cyberpunk-checkbox-label">
          <input
            type="checkbox"
            class="cyberpunk-checkbox"
            v-model="compConfig.list[k].value"
            @change="checkValueChange(k)"
          />
          {{ v.item }}</label
        >
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
    @include global.wh(100%, auto);
    text-align: start;
    word-break: break-all;
    @include global.font_config(1.2vw, rgba(51, 51, 51, 0.8));
    white-space: pre-wrap; /* CSS 2.1 */
    white-space: -moz-pre-wrap; /* Mozilla, since 1999 */
    white-space: -pre-wrap; /* Opera 4-6 */
    white-space: -o-pre-wrap; /* Opera 7 */
    // word-wrap: break-word; /* Internet Explorer 5.5+ */
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
.cyberpunk-checkbox {
  appearance: none;
  width: 20px;
  height: 20px;
  border: 2px solid #666c72a1;
  border-radius: 5px;
  background-color: transparent;
  display: inline-block;
  position: relative;
  margin-right: 10px;
  cursor: pointer;
}

.cyberpunk-checkbox:before {
  content: '';
  background-color: #666c72a1;
  display: block;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0);
  width: 10px;
  height: 10px;
  border-radius: 3px;
  transition: all 0.3s ease-in-out;
}

.cyberpunk-checkbox:checked:before {
  transform: translate(-50%, -50%) scale(1);
}

.cyberpunk-checkbox-label {
  font-size: 1.4vw;
  cursor: pointer;
  user-select: none;
  display: flex;
  align-items: center;
}
</style>
