<script setup lang="ts">
import { reactive, watch } from 'vue'

// 默认配置
const defaultConfig = {
  switchValue: 0,
  switchText: 'text',
  switchDesc: 'desc',
}

// 默认样式
const defaultPattern = {
  textMargin: '20px',
  boxMargin: '20px',
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
const switchConfig = reactive({ ...defaultConfig, ...props.config })

watch(
  switchConfig,
  () => {
    emit('update:config', { ...switchConfig })
  },
  { deep: true }
)

/********************************************************************************
 * @brief: 样式副本
 ********************************************************************************/
const switchPattern = reactive({ ...defaultPattern, ...props.pattern })
</script>

<template>
  <div
    class="switch-tool-content"
    :style="{ marginBottom: switchPattern.boxMargin }"
  >
    <div
      class="first-line"
      :style="{ marginBottom: switchPattern.textMargin }"
    >
      <div class="switch-text">{{ switchConfig.switchText }}</div>
      <div class="switch-input">
        <input
          type="checkbox"
          name="checkbox-input"
          class="checkbox-input"
          :id="`${switchConfig.switchText}`"
          v-model="switchConfig.switchValue"
        />
        <label
          :for="`${switchConfig.switchText}`"
          class="label"
        >
        </label>
      </div>
    </div>
    <div class="switch-desc">
      {{ switchConfig.switchDesc }}
    </div>
  </div>
</template>

<style scoped lang="scss">
.switch-tool-content {
  @include global.wh(100%, auto);
  @include global.flex_config(1, space-between);
  .first-line {
    @include global.wh(100%, 50px);
    @include global.flex_config(0, space-between);
    @include global.font_config(1.5vw, rgba(51, 51, 51, 1));
    // margin-bottom: 12px;
  }
  .switch-desc {
    @include global.wh(100%, 50px);
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
