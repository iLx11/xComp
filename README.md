# switch

### 父组件

```vue
<script setup lang="ts">
import { reactive } from 'vue'
const switchData = reactive({
  // 数值
  value: false,
  // 配置名
  text: 'text',
  // 配置描述
  desc: 'desc',
})

const switchPattern = reactive({
  // text 与 desc 底部间距
  textMargin: '20px',
  // 配置组件与其他组件底部间距
  boxMargin: '20px',
})
</script>
<template>
  <SwitchTool
    v-model:config="switchData"
    :pattern="switchPattern"
  />
</template>
```

# select

## drop down

### 父组件

```vue
<script setup lang="ts">
import { reactive } from 'vue'
const selectDropData = reactive({
  // 数值
  list: [
    {
      item: 'item',
      value: false,
    },
  ],
  // 配置名
  text: 'text',
  // 配置描述
  desc: 'desc',
})

const selectPattern = reactive({
  // text 与 desc 底部间距
  textMargin: '20px',
  // 配置组件与其他组件底部间距
  boxMargin: '20px',
})
</script>
<template>
  <SelectDropTool
    v-model:config="selectDropData"
    :pattern="selectPattern"
  />
</template>
```

# range

### 普通组件

```vue
<script setup lang="ts">
import { reactive } from 'vue'
const rangeData = reactive({
  // 数值
  value: 0,
  // 最大（范围）
  max: 100,
  // 最小（范围）
  min: 0,
  // 步进
  step: 10,
  // 配置名
  text: 'text',
  // 配置描述
  desc: 'desc',
})

const rangePattern = reactive({
  // 移动快左位移
  moveLeft: '0%',
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
})
</script>
<template>
  <RangeTool
    :config="rangeData"
    @update:config="valueUpdate($event, rangeData)"
  />
</template>
```

### 刻度 range

```vue
<script setup lang="ts">
import { reactive } from 'vue'
const rangeData = reactive({
  // 数值
  value: 0,
  // 最大（范围）
  max: 100,
  // 最小（范围）
  min: 0,
  // 步进
  step: 10,
  // 配置名
  text: 'text',
  // 配置描述
  desc: 'desc',
  // 刻度
  scale: 10,
})

const rangePattern = reactive({
  // 移动快左位移
  moveLeft: '0%',
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
})
</script>
<template>
  <RangeTool
    :config="rangeData"
    @update:config="valueUpdate($event, rangeData)"
  />
</template>
```

# checkbox

### 普通组件

```vue
<script setup lang="ts">
import { reactive } from 'vue'
const checkboxData = reactive([
  {
    // 数值
    list: [
      {
        item: '1.0mm',
        value: false,
      },
      {
        item: '2.0mm',
        value: false,
      },
    ],
    // 配置名
    text: '鼠标 LOD 高度',
    // 配置描述
    desc: '鼠标离开桌面停止运动的距离，低：1mm，高：2mm',
    // 是否单选
    single: true,
  },
])

const checkboxPattern = reactive({
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
})

/********************************************************************************
 * @brief: 更新配置值
 * @param {*} newObj 更新的值
 * @param {*} config 配置值
 * @return {*}
 ********************************************************************************/
const valueUpdate = (newObj: Object, config: Object) => {
  Object.keys(config).forEach(key => {
    if (key in newObj) {
      config[key] = newObj[key]
    }
  })
}
</script>
<template>
  <CheckboxTool
    :config="checkboxData"
    @update:config="valueUpdate($event, checkboxData)"
  />
</template>
```

### 功能组件

```vue
<script setup lang="ts">
import { reactive } from 'vue'
const checkboxData = reactive([
  {
    // 数值
    list: [
      {
        item: '1.0mm',
        value: false,
      },
      {
        item: '2.0mm',
        value: false,
      },
    ],
    // 配置名
    text: '鼠标 LOD 高度',
    // 配置描述
    desc: '鼠标离开桌面停止运动的距离，低：1mm，高：2mm',
    // 是否单选
    single: true,
    // 是否可编辑
    writable: false,
  },
])

const checkboxPattern = reactive({
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
})

/********************************************************************************
 * @brief: 更新配置值
 * @param {*} newObj 更新的值
 * @param {*} config 配置值
 * @return {*}
 ********************************************************************************/
const valueUpdate = (newObj: Object, config: Object) => {
  Object.keys(config).forEach(key => {
    if (key in newObj) {
      config[key] = newObj[key]
    }
  })
}
</script>
<template>
  <CheckboxTool
    :config="checkboxData"
    @update:config="valueUpdate($event, checkboxData)"
  />
</template>
```
