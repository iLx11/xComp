<script setup lang="ts">
import { reactive, watch } from 'vue'

// 默认配置
const defaultConfig = {
  value: false,
  text: 'text',
  desc: 'desc',
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
    :style="{ marginBottom: compPattern.boxMargin }"
  >
    <div
      class="first-line"
      :style="{ marginBottom: compPattern.textMargin }"
    >
      <div class="switch-text">
        {{ check(compProps.text, compConfig.text) }}
      </div>
      <div class="switch-input">
        <div class="toggle-container">
          <label
            :for="`${check(compProps.text, compConfig.text)}`"
            class="label"
          >
          <input
            checked=""
            type="checkbox"
            class="toggle-input"
            v-model="compProps.value"
            :id="`${check(compProps.text, compConfig.text)}`"
          />
          
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 292 142"
              class="toggle"
            >
              <path
                d="M71 142C31.7878 142 0 110.212 0 71C0 31.7878 31.7878 0 71 0C110.212 0 119 30 146 30C173 30 182 0 221 0C260 0 292 31.7878 292 71C292 110.212 260.212 142 221 142C181.788 142 173 112 146 112C119 112 110.212 142 71 142Z"
                class="toggle-background"
              ></path>
              <rect
                rx="6"
                height="64"
                width="12"
                y="39"
                x="64"
                class="toggle-icon on"
              ></rect>
              <path
                d="M221 91C232.046 91 241 82.0457 241 71C241 59.9543 232.046 51 221 51C209.954 51 201 59.9543 201 71C201 82.0457 209.954 91 221 91ZM221 103C238.673 103 253 88.6731 253 71C253 53.3269 238.673 39 221 39C203.327 39 189 53.3269 189 71C189 88.6731 203.327 103 221 103Z"
                fill-rule="evenodd"
                class="toggle-icon off"
              ></path>
              <g filter="url('#goo')">
                <rect
                  fill="#fff"
                  rx="29"
                  height="58"
                  width="116"
                  y="42"
                  x="13"
                  class="toggle-circle-center"
                ></rect>
                <rect
                  fill="#fff"
                  rx="58"
                  height="114"
                  width="114"
                  y="14"
                  x="14"
                  class="toggle-circle left"
                ></rect>
                <rect
                  fill="#fff"
                  rx="58"
                  height="114"
                  width="114"
                  y="14"
                  x="164"
                  class="toggle-circle right"
                ></rect>
              </g>
              <filter id="goo">
                <feGaussianBlur
                  stdDeviation="10"
                  result="blur"
                  in="SourceGraphic"
                ></feGaussianBlur>
                <feColorMatrix
                  result="goo"
                  values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7"
                  mode="matrix"
                  in="blur"
                ></feColorMatrix>
              </filter>
            </svg>
          </label>
        </div>
      </div>
    </div>
    <div class="switch-desc">
      {{ check(compProps.desc, compConfig.desc) }}
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

/* From Uiverse.io by njesenberger */
.toggle-container {
  --active-color: #2c3e50;
  --inactive-color: #d3d3d6;
  position: relative;
  aspect-ratio: 292 / 142;
  height: 1.875em;
  @include global.flex_center;
}

.toggle-input {
  appearance: none;
  margin: 0;
  position: absolute;
  z-index: 1;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  cursor: pointer;
}

.toggle {
  width: 80%;
  height: 80%;
  overflow: visible;
}

.toggle-background {
  fill: var(--inactive-color);
  transition: fill 0.4s;
}

.toggle-input:checked + .toggle .toggle-background {
  fill: var(--active-color);
}

.toggle-circle-center {
  transform-origin: center;
  transition: transform 0.6s;
}

.toggle-input:checked + .toggle .toggle-circle-center {
  transform: translateX(150px);
}

.toggle-circle {
  transform-origin: center;
  transition: transform 0.45s;
  backface-visibility: hidden;
}

.toggle-circle.left {
  transform: scale(1);
}

.toggle-input:checked + .toggle .toggle-circle.left {
  transform: scale(0);
}

.toggle-circle.right {
  transform: scale(0);
}

.toggle-input:checked + .toggle .toggle-circle.right {
  transform: scale(1);
}

.toggle-icon {
  transition: fill 0.4s;
}

.toggle-icon.on {
  fill: var(--inactive-color);
}

.toggle-input:checked + .toggle .toggle-icon.on {
  fill: #fff;
}

.toggle-icon.off {
  fill: #eaeaec;
}

.toggle-input:checked + .toggle .toggle-icon.off {
  fill: var(--active-color);
}
</style>
