<script setup lang="ts">
import { reactive, watch, ref } from 'vue'

// 默认配置
const defaultConfig = {
  list: [],
  text: 'text',
  desc: 'desc',
  single: false,
  writable: false,
  // 可取反
  reversible: false,
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
  // 盒子样式
  contentBoxStyle: {},
  // 盒子选中样式
  boxSelectStyle: {},
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
  // 判断是否可取反
  if (check(compProps.reversible, compConfig.reversible)) {
    compConfig.list[index].value = !compConfig.list[index].value
  } else {
    compConfig.list[index].value = true
  }
  // 取消 input 没有选中后的焦点
  if (
    check(compProps.writable, compConfig.writable) &&
    !compConfig.list[index].value
  ) {
    boxContentRef.value.children[index].children[0].blur()
  }
}

const boxContentRef = ref()

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
      ref="boxContentRef"
    >
      <div
        v-for="(v, k) in compProps.list"
        :key="v"
        @click="checkValueChange(k)"
        :style="compPattern.contentBoxStyle"
      >
        <!-- 选中样式 -->
        <div
          class="select-style-box"
          v-if="compProps.list[k].value"
          :style="compPattern.boxSelectStyle"
        ></div>
        <div
          v-if="!check(compProps.writable, compConfig.writable)"
          :style="{
            color: v.value ? 'rgba(255, 255, 255, 0.85)' : '',
          }"
        >
          {{ v.item }}
        </div>
        <input
          type="number"
          v-model="compConfig.list[k].item"
          v-else
        />
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
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
      position: relative;
      overflow: hidden;
      input {
        @include global.full_wh;
        outline: none;
        border: none;
        background-color: rgba(255, 255, 255, 0);
        text-align: center;
        @include global.font_config(1.2vw, rgba(51, 51, 51, 0.8));
      }
    }
    .select-style-box {
      @include global.full_wh;
      @include global.pos_ab;
      background: rgb(176, 195, 209);
      z-index: -1;
    }
  }
}
</style>
