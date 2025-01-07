<script setup lang="ts">
import { onMounted, reactive, ref, watch } from 'vue'

// 默认配置
const defaultConfig = {
  text: '选择文件',
  value: '',
  writable: true,
  disable: false,
}

// 默认样式
const defaultPattern = {
  // 配置组件与其他组件底部间距
  boxMargin: '20px',
  // text 字体大小
  textFontSize: '1.5vw',
  // 组件 padding
  boxPadding: '0',
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

const emit = defineEmits(['update:config', 'choose'])
/********************************************************************************
 * @brief: 配置副本
 ********************************************************************************/
const compConfig = reactive({ ...defaultConfig, ...props.config })
const compProps = reactive(props.config)

/********************************************************************************
 * @brief: 样式副本
 ********************************************************************************/
const compPattern = reactive({ ...defaultPattern, ...props.pattern })

const dataChangeState = ref<number>(0)

watch(
  compConfig,
  () => {
    if (dataChangeState.value == 2) {
      dataChangeState.value = 0
      return
    }
    if (dataChangeState.value == 0) {
      dataChangeState.value = 1
      // console.info('down change')
      // 如果不是上层更改则更新到上层
      emit('update:config', { ...compConfig })
    }
  },
  { deep: true }
)

// 上层数据更改
watch(
  props.config,
  () => {
    if (dataChangeState.value == 1) {
      dataChangeState.value = 0
      return
    }
    if (dataChangeState.value == 0) {
      dataChangeState.value = 2
      // console.info('up change')
      // 更新下层数据
      Object.keys(compConfig).forEach((key) => {
        if (key in props.config) {
          compConfig[key] = props.config[key]
        }
      })
    }
  },
  { deep: true }
)

const chooseFile = () => {
  emit('choose', { ...compConfig })
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
    class="choose-file-box"
    :style="{
      marginBottom: compPattern.boxMargin,
      padding: compPattern.boxPadding,
      fontSize: compPattern.textFontSize,
    }"
  >
    <div
      class="disable-cover"
      v-if="check(compProps.disable, compConfig.disable)"
    ></div>
    <div
      class="choose-button"
      @click="chooseFile"
    >
      {{ check(compProps.text, compConfig.text) }}
    </div>
    <input
      type="text"
      name=""
      id=""
      v-model="compConfig.value"
    />
  </div>
</template>

<style lang="scss" scoped>
.choose-file-box {
  @include global.wh(100%, 50px);
  @include global.flex_config(0, space-between);
  @include global.style_common(16px, null, null, global.$shadow1);
  margin-bottom: 40px;
  overflow: hidden;
  position: relative;
  .disable-cover {
    @include global.full_wh;
    @include global.pos_ab;
    cursor: not-allowed;
  }
  .choose-button {
    @include global.wh(32%, 100%);
    @include global.flex_center;
    font-size: 2.2vw;
    cursor: pointer;
  }
  input {
    @include global.wh(68%, 100%);
    outline: none;
    background-color: rgb(234, 234, 234);
    border: none;
    padding: 0 10px;
    color: rgba(51, 51, 51, 0.5);
  }
}
</style>
