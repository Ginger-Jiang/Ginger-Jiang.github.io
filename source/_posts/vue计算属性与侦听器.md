---
title: vue计算属性与侦听器
date: 2020-02-03 00:59:57
tags:
  - Vue
categories:
  - Vue
---

## 计算属性

虽然我们可以在模板内使用表达式, 但是设计初衷是用于简单的运算, 如果在模板中放入太多的逻辑会让模板过重难以维护

```html
<div>{{ message.split('').reverse().join() }}</div>
```

### 例子

```html
<template>
  <div id="app">
    <p>原始message: {{ message }}</p>
    <p>计算属性message: {{ newMessage }}</p>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        message: 'Hello World'
      }
    },
    computed: {
      newMessage() {
        return this.message
          .split('')
          .reverse()
          .join('')
      }
    }
  }
</script>
```

## 计算属性缓存 vs 方法

如果我们在`methods`中定义方法, 在模板中调用, 一样可以实现这个功能  
不同的是, 计算属性是基于他们的响应式依赖进行缓存的, 只有在相关响应式依赖发生改变时他们才会重新计算求值

```html
<template>
  <div id="app">
    <p>原始message: {{ message }}</p>
    <p>计算属性message: {{ reverseStr() }}</p>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        message: 'Hello World'
      }
    },
    methods: {
      reverseStr() {
        return this.message
          .split('')
          .reverse()
          .join('')
      }
    }
  }
</script>
```

### 计算属性的 getter

计算属性默认只有 getter, 不过我们在需要时也可以提供一个 setter:

```html
<template>
  <div id="app">
    <p>{{ firstName }}</p>
    <p>{{ lastName }}</p>
    <p v-if="fullNmae">{{ fullNmae }}</p>
    <button @click="()=>{fullNmae = 'Hi,Ginger'}">update fullName</button>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        firstName: 'Hello',
        lastName: 'World'
      }
    },
    computed: {
      fullNmae: {
        get() {
          return this.firstName + this.lastName
        },
        set(newVal) {
          let arr = newVal.split(',')
          this.firstName = arr[0]
          this.lastName = arr[1]
        }
      }
    }
  }
</script>
```

## 侦听器

Vue 通过`watch`选项提供了一个可自定义的侦听器来响应数据的变化, 当需要在数据变化时执行异步或开销较大的操作时, 适合使用过

### 例子

```html
<template>
  <div id="app">
    <p>{{ content }}</p>
    <button @click="onClick">page++</button>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        content: '', // 页面内容
        page: 1 // 页码
      }
    },
    methods: {
      onClick() {
        this.page++
      }
    },
    watch: {
      page(newVal) {
        // 根据页码发起数据请求 获取最新页数数据
        ...
      }
    }
  }
</script>
```

## 计算属性 vs 侦听属性

Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动: `侦听属性`.  
不过官方更推荐使用计算属性

```html
<template>
  <div id="app">
    <p>{{ fullName}}</p>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        firstName: 'Hello',
        lastName: 'World',
        fullName: ''
      }
    },
    created() {
      this.firstName = 'Hello'
    },
    // 侦听器实现 重复代码
    watch: {
      firstName(val) {
        return (this.fullName = val + this.lastName)
      },
      lastName(val) {
        return (this.fullName = this.firstName + val)
      }
    },
    // 计算属性实现 更简洁
    computed: {
      fullName() {
        return this.firstName + this.lastName
      }
    }
  }
</script>
```
