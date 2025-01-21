<script setup lang="ts">
import { onMounted, reactive, watch, ref } from 'vue'

onMounted(() => {
  defaultText.value = compConfig.list[compConfig.list.findIndex(o => o.value == true)].item
})

// 默认配置
const defaultConfig = {
  list: [],
  text: 'text',
  desc: 'desc',
}

// 默认样式
const defaultPattern = {
  // 输入组件宽度
  inputWidth: '50%',
  // text 与 desc 底部间距
  textMargin: '20px',
  // 配置组件与其他组件底部间距
  boxMargin: '20px',
  // text 字体大小
  textFontSize: '1.5vw',
  // select 组件字体大小
  selectFontSize: '1.2vw',
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

// onMounted(() => {
//   defaultText.value = compConfig.list[0] && compConfig.list[0].item
// })

// 默认选项配置
const defaultText = ref<string>('')

/********************************************************************************
 * @brief: 选择子项目
 * @param {*} index
 * @return {*}
 ********************************************************************************/
const selectOption = (index: number) => {
  compConfig.list.forEach((o) => (o.value = false))
  compConfig.list[index].value = true
  defaultText.value = compConfig.list[index].item
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
    class="select-tool-content"
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
        class="select-text"
        :style="{ fontSize: compPattern.textFontSize }"
      >
        {{ check(compProps.text, compConfig.text) }}
      </div>
      <div
        class="select-input"
        :style="{ width: compPattern.inputWidth, fontSize: compPattern.selectFontSize  }"
      >
        <div class="dropdown">
          <input
            type="checkbox"
            class="dropdown__select"
            :id="`${check(compProps.text, compConfig.text)}`"
            hidden
          />
          <label
            :for="`${check(compProps.text, compConfig.text)}`"
            class="dropdown__options-filter"
          >
            <ul
              class="dropdown__filter"
              role="listbox"
              tabindex="-1"
            >
              <li
                class="dropdown__filter-selected"
                aria-selected="true"
              >
                {{ defaultText }}
              </li>
              <li>
                <ul class="dropdown-ul dropdown__select">
                  <li
                    class="dropdown__select-option"
                    role="option"
                    v-for="(v, k) in check(compProps.list, compConfig.list)"
                    :key="v.item"
                    @click="selectOption(k)"
                  >
                    {{ v.item }}
                  </li>
                </ul>
              </li>
            </ul>
          </label>
        </div>
      </div>
    </div>
    <div
      class="select-desc"
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
.select-tool-content {
  @include global.wh(100%, auto);
  @include global.flex_config(1, space-between);
  .first-line {
    @include global.wh(100%, auto);
    @include global.flex_config(0, space-between);
    flex-wrap: wrap;
    @include global.font_config(1.5vw, rgba(51, 51, 51, 1));
    gap: 2vw;
    // margin-bottom: 12px;
  }
  .select-desc {
    @include global.wh(100%, auto);
    text-align: start;
    word-break: break-all;
    @include global.font_config(1.2vw, rgba(51, 51, 51, 0.8));
  }
  // margin-bottom: 12px;
  padding-right: 0.6em;
  padding-top: 0.8em;
}

// .select-input {
//   // @include global.wh(50%, 100%);
//   height: 100%;
//   font-size: 1.2vw !important;
// }

.dropdown {
  // font-family: 'Helvetica', sans-serif;
  // font-weight: 300;
  box-shadow: 0 0 0 2px rgba(51, 51, 51, 0.2);
  border-radius: 12px;
  @include global.style_common(16px, null, null, global.$shadow1);
  &__select:checked + &__options-filter &__select {
    transform: scaleY(1);
    opacity: 1;
  }

  &__select:checked + &__options-filter &__filter:after {
    transform: rotate(-135deg);
  }

  &__options-filter {
    width: 100%;
    cursor: pointer;
  }

  &__filter {
    position: relative;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 0 0 0 20px;
    height: 50px;
    color: #595959;
    background-color: #fff;
    border-radius: 18px;
    // font-size: 14px;
    transition: 0.3s;
    z-index: 9;
    min-width: 0;
    min-height: 0;
    box-sizing: border-box;

    &:focus {
      outline: none;
    }

    &::after {
      position: absolute;
      top: 45%;
      right: 20px;
      content: '';
      width: 10px;
      height: 10px;
      border-right: 1px solid #595959;
      border-bottom: 1px solid #595959;
      transform: rotate(45deg) translateX(-45%);
      transition: 0.3s ease-in-out;
    }
  }

  &__select {
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    max-height: 150px;
    margin-top: 12px;
    overflow: scroll;
    transform: scaleY(0);
    transform-origin: top;
    border-radius: 18px;
    opacity: 0;
    transition: 0.2s ease-in-out;
    @include global.style_common(16px, null, null, global.$shadow1);
  }

  &__select-option {
    padding: 20px;
    background-color: #fff;
    border-bottom: 1px solid #e9ecef;
    transition: 0.3s;

    &:last-of-type {
      border-bottom: 0;
    }

    &:hover {
      background-color: #e9ecef;
    }
  }
}
</style>
