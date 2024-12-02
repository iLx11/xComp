# switch

### 父组件

```vue
<script setup lang="ts">
import { reactive } from 'vue'
const switchData = reactive(
	{
        // 数值
        value: false,
        // 配置名
        text: 'text',
        // 配置描述
        desc: 'desc',
	}
)

const switchPattern = reactive(
	{
        // text 与 desc 底部间距
    	textMargin: '20px',
        // 配置组件与其他组件底部间距
  		boxMargin: '20px',
    }
)
</script>
<template>
    <SwitchTool
       v-model:config="switchData" 
       :pattern="switchPattern"
    />
</<template>>
```

# select

## drop down

### 父组件

```vue
<script setup lang="ts">
import { reactive } from 'vue'
const selectData = reactive(
	{
        // 数值
        list: [
            {
                item: 'item',
                value: false
            }
        ],
        // 配置名
        text: 'text',
        // 配置描述
        desc: 'desc',
	}
)

const selectPattern = reactive(
	{
        // text 与 desc 底部间距
    	textMargin: '20px',
        // 配置组件与其他组件底部间距
  		boxMargin: '20px',
    }
)
</script>
<template>
    <SelectTool
       v-model:config="selectData" 
       :pattern="selectPattern"
    />
</<template>>
```

# range

### 父组件

```vue
<script setup lang="ts">
import { reactive } from 'vue'
const rangeData = reactive(
	{
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
	}
)

const rangePattern = reactive(
	{
        // 移动快左位移
    	moveLeft: '0%',
    }
)
</script>
<template>
    <SwitchTool
       v-model:config="rangeData" 
       :pattern="rangePattern"
    />
</<template>>
```

# checkbox

### 父组件

```vue
<script setup lang="ts">
import { reactive } from 'vue'
const rangeData = reactive(
	{
        // 配置列表
        list: [
            {
                item: '',
                value: false
            }
        ],
        // 配置名
        text: 'text',
        // 配置描述
        desc: 'desc',
	}
)

const rangePattern = reactive(
	{
        // text 与 desc 底部间距
    	textMargin: '20px',
        // 配置组件与其他组件底部间距
  		boxMargin: '20px',
    }
)
</script>
<template>
    <SwitchTool
       v-model:config="rangeData" 
       :pattern="rangePattern"
    />
</<template>>
```

