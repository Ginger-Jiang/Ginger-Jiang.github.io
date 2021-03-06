---
title: vue表单输入绑定
date: 2020-02-04 20:02:28
tags:
  - Vue
categories:
  - Vue
---

## 基础用法

你可以用 `v-model` 指令在表单 `<input>`、`<textarea>`、`<select>` 元素上创建双向数据绑定. 它会根据控件类型自动选取正确的方法来更新元素. 但`v-model`的本质只是语法糖

> `v-model` 会忽略所有表单元素的 `vaule`、`checked`、`select`的初始值, 而使用 Vue 实例中数据作为数据来源, 所以我们需要在 data 中声明初始值

`v-model` 在内部为不同的表单元素使用不同的属性并抛出不同的事件:

- text 和 textarea 元素使用 `value` 属性和 `input` 事件
- checkbox 和 radio 使用 `checked` 属性和 `change` 事件
- select 字段将 `value` 作为 prop 并将 `change` 作为事件

### 文本

```html
<template>
  <div class="hello">
    <input type="text" v-model="msg" />
    <textarea v-model="msg"></textarea>
    <p>{{ msg }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        msg: 'Hi'
      }
    }
  }
</script>
```

### 复选框

```html
<template>
  <div class="hello">
    <!-- 单个复选框 -->
    <input type="checkbox" id="checkbox" v-model="checked" />
    <label for="checkbox"></label>
    <p>{{ checked }}</p>
    <hr />
    <!-- 多个复选框 -->
    <input type="checkbox" id="Jack" value="Jack" v-model="checkes" />
    <input type="checkbox" value="Ginger" v-model="checkes" />
    <input type="checkbox" value="Som" v-model="checkes" />
    <p>{{ checkes }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        checked: false,
        checkes: []
      }
    }
  }
</script>
```

### 单选框

```html
<template>
  <div class="hello">
    <label for="y">
      Yes
      <input type="radio" id="y" value="y" v-model="res" />
    </label>
    <label for="n">
      No
      <input type="radio" id="n" value="n" v-model="res" />
    </label>
    <p>{{ res }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        res: ''
      }
    }
  }
</script>
```

### 选择框

```html
<template>
  <div class="hello">
    <select v-model="selectd">
      <option disabled value>点击选择</option>
      <option>A</option>
      <option>B</option>
      <option>C</option>
    </select>
    <p>{{ selectd }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        selectd: ''
      }
    }
  }
</script>
```

## 修饰符

### `.lazy` 修饰符

`v-model` 会在每次 `input` 事件触发后将输入框的值与数据进行同步, 可以通过添加 `.lazy` 修饰符, 从而转变为使用 `change` 事件进行同步

```html
<template>
  <div class="hello">
    <input v-model.lazy="msg" />
    <br />
    <span>change update: {{ msg }}</span>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        msg: ''
      }
    }
  }
</script>
```

### `.number` 修饰符

如果想将用户输入值转为数值类型, 可以给 `v-model` 添加 `number` 修饰符

```html
<!-- string => number -->
<input v-model.number="age" type="number" />
```

### `.trim`

如果要自动过滤用户输入的收尾空白字符, 可以给 `v-model` 添加 `trim` 修饰符

```html
<input v-model.trim="msg" />
```

## 在组件上使用 v-model

在原生 HTML 的输入元素不能满足需求时, Vue 允许创建自定义行为的输入组件, 并且可以绑定 `v-model` 一起使用
