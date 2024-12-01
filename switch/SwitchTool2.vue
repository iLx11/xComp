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
        <label class="switch-label">
          <input
            class="toggle-checkbox"
            type="checkbox"
          />
          <div class="toggle-slot">
            <div class="sun-icon-wrapper">
              <div
                class="iconify sun-icon"
                data-icon="feather-sun"
                data-inline="false"
              ></div>
            </div>
            <div class="toggle-button"></div>
            <div class="moon-icon-wrapper">
              <div
                class="iconify moon-icon"
                data-icon="feather-moon"
                data-inline="false"
              ></div>
            </div>
          </div>
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

.toggle-checkbox {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.toggle-slot {
  font-size: 10px;
  position: relative;
  height: 3.5em;
  width: 7em;
  border: 0px solid transparent;
  border-radius: 10em;
  background-color: white;
  transition: background-color 250ms;
  box-shadow: 0 0 0 2px rgba(51, 51, 51, 0.3);
  margin-right: 2px;
}

.toggle-checkbox:checked ~ .toggle-slot {
  background-color: #374151;
  box-shadow: 0 0 0 0px rgba(51, 51, 51, 0.3);
}

.toggle-button {
  transform: translate(0.3em, 0.25em);
  position: absolute;
  height: 3em;
  width: 3em;
  border-radius: 50%;
  background-color: #ffeccf;
  box-shadow: inset 0px 0px 0px 0.75em #ffbb52;
  transition: background-color 250ms, border-color 250ms,
    transform 500ms cubic-bezier(0.26, 2, 0.46, 0.71);
}

.toggle-checkbox:checked ~ .toggle-slot .toggle-button {
  background-color: #485367;
  box-shadow: inset 0px 0px 0px 0.75em white;
  transform: translate(3.65em, 0.25em);
}

.sun-icon {
  position: absolute;
  height: 6em;
  width: 6em;
  color: #ffbb52;
}

.sun-icon-wrapper {
  position: absolute;
  height: 6em;
  width: 6em;
  opacity: 1;
  transform: translate(2em, 2em) rotate(15deg);
  transform-origin: 50% 50%;
  transition: opacity 150ms, transform 500ms cubic-bezier(0.26, 2, 0.46, 0.71);
}

.toggle-checkbox:checked ~ .toggle-slot .sun-icon-wrapper {
  opacity: 0;
  transform: translate(3em, 2em) rotate(0deg);
}

.moon-icon {
  position: absolute;
  height: 6em;
  width: 6em;
  color: white;
}

.moon-icon-wrapper {
  position: absolute;
  height: 6em;
  width: 6em;
  opacity: 0;
  transform: translate(11em, 2em) rotate(0deg);
  transform-origin: 50% 50%;
  transition: opacity 150ms, transform 500ms cubic-bezier(0.26, 2.5, 0.46, 0.71);
}

.toggle-checkbox:checked ~ .toggle-slot .moon-icon-wrapper {
  opacity: 1;
  transform: translate(2em, 2em) rotate(-15deg);
}
</style>
