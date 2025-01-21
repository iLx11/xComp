<script setup lang="ts">
import { reactive, watch } from 'vue'

// 默认配置
const defaultConfig = {
  value: 0,
  text: 'text',
  desc: 'desc',
}

// 默认样式
const defaultPattern = {
  // text 与 desc 底部间距
  textMargin: '20px',
  // 配置组件与其他组件底部间距
  boxMargin: '20px',
  // text 字体大小
  textFontSize: '1.5vw',
  // 组件 padding
  boxPadding: '0',
  // 子盒子间隔
  boxGap: '10px',
  // 标题内容布局 (start, center, end)
  textAlign: 'start',
  // 描述字符高度
  descLine: 1.2,
  // 描述底部间距
  descMargin: '20px',
  // 描述字体大小
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
    class="switch-tool-content"
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
        class="switch-text"
        :style="{ fontSize: compPattern.textFontSize }"
      >
        {{ check(compProps.text, compConfig.text) }}
      </div>
      <div class="switch-input">
        <input
          type="checkbox"
          name="checkbox-input"
          class="checkbox-input"
          :id="`${check(compProps.text, compConfig.text)}`"
          v-model="compProps.value"
        />
        <label
          :for="`${check(compProps.text, compConfig.text)}`"
          class="label"
        >
        </label>
      </div>
    </div>
    <div
      class="switch-desc"
      :style="{
        lineHeight: compPattern.descLine,
        marginBottom: compPattern.descMargin,
        fontSize: compPattern.descFontSize,
      }"
    >
      {{ check(compProps.desc, compConfig.desc) }}
    </div>
  </div>
</template>

<style scoped lang="scss">
.switch-tool-content {
  @include global.wh(100%, auto);
  @include global.flex_config(1, space-between);
  .first-line {
    @include global.wh(100%, auto);
    @include global.flex_config(0, space-between);
    @include global.font_config(1.5vw, rgba(51, 51, 51, 1));
    // margin-bottom: 12px;
  }
  .switch-desc {
    @include global.wh(100%, auto);
    text-align: start;
    word-break: break-all;
    @include global.font_config(1.2vw, rgba(51, 51, 51, 0.8));
  }
  // margin-bottom: 12px;
}
.label {
  height: 50px;
  width: 100px;
  background-color: #ffffff;
  border-radius: 30px;
  // -webkit-box-shadow: inset 0 0 5px 4px rgba(255, 255, 255, 1),
  //   inset 0 0 20px 1px rgba(0, 0, 0, 0.488), 10px 20px 30px rgba(0, 0, 0, 0.096),
  //   inset 0 0 0 3px rgba(0, 0, 0, 0.3);
  // box-shadow: inset 0 0 5px 4px rgba(255, 255, 255, 1),
  //   inset 0 0 20px 1px rgba(0, 0, 0, 0.488), inset 0 0 0 3px rgba(0, 0, 0, 0.3);
  box-shadow: inset 0 0 5px 4px rgba(255, 255, 255, 1),
    inset 0 0 5px 1px rgba(0, 0, 0, 0.3), inset 0 0 0 3px rgba(0, 0, 0, 0.3);
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  cursor: pointer;
  position: relative;
  -webkit-transition: -webkit-transform 0.4s;
  transition: -webkit-transform 0.4s;
  transition: transform 0.4s;
}

.label:hover {
  -webkit-transform: perspective(80px) rotateX(5deg) rotateY(-5deg);
  transform: perspective(80px) rotateX(5deg) rotateY(-5deg);
}

.checkbox-input:checked ~ label:hover {
  -webkit-transform: perspective(80px) rotateX(-5deg) rotateY(5deg);
  transform: perspective(80px) rotateX(-5deg) rotateY(5deg);
}

.checkbox-input {
  display: none;
}

.checkbox-input:checked ~ label::before {
  left: 60px;
  background-color: #5c5c5c;
  background-image: linear-gradient(
    315deg,
    rgba(51, 51, 51, 0.1) 0%,
    rgba(143, 142, 142, 0.4) 70%
  );
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

// 按钮圆
.label::before {
  position: absolute;
  content: '';
  height: 30px;
  width: 30px;
  border-radius: 50%;
  background-color: #5c5c5c;
  background-image: linear-gradient(
    130deg,
    rgba(255, 255, 255, 1) 10%,
    #ffffff 11%,
    rgb(168, 166, 166) 62%
  );
  left: 10px;
  -webkit-box-shadow: 0 2px 1px rgba(51, 51, 51, 0.2),
    5px 5px 5px rgba(51, 51, 51, 0.2);
  box-shadow: 0 2px 1px rgba(51, 51, 51, 0.2), 5px 5px 5px rgba(51, 51, 51, 0.2);
  -webkit-transition: 0.4s;
  transition: 0.4s;
}
</style>
