# el-readonly 常用表单输入控件转文字组件

## 📦 What

基于vue + element-ui的常用表单输入控件转文字组件

## 🚀 Why

在表单场景中，会出现非常多的各类型输入控件，然而表单又通常会出现编辑、查看两种状态，于是就涉及了输入控件的只读模式，如果对于普通输入框，仅使用if else也能解决，但对于select及更复杂的组件，可能还会涉及到值遍历。我们通过一些魔术方法，实际上完全可以轻松地对输入控件进行转换。

```vue
<template>
    <div>
      <Readonly :enabled="true">
        <Input v-model="inputValue">
      </Readonly>
      <Readonly :enabled="true">
        <Select v-model="selectValue">
          <Option label="苹果" value="apple" />
          <Option label="香蕉" value="banana" />
        </Select>
      </Readonly>
    </div>
</template>

<script>
import Readonly from 'el-readonly';
import {Input, Select, Option} from 'element-ui';

export default {
  components: {Readonly, Input, Select, Option},
  data(){
    return {
      inputValue: '输入一些内容',
      selectValue: 'apple',
    }
  }
} 
</script>
```

![](https://i.imgur.com/b00lNuQ.jpg)

# 🎮 How

`npm install el-readonly -S`

**enabled**: 是否启用的开关。

**formatter**: 自定义值显示的方法。

**default-text**: 值为空时的默认展示字符，默认值为`-`。

**ellipsis**: 是否单行超出时展示省略号。