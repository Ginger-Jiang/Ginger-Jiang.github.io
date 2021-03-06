---
title: vue事件处理
date: 2020-02-04 11:19:19
tags:
  - Vue
categories:
  - Vue
---

## 监听事件

可以用 `v-on` 指令监听 DOM 事件, 并在触发时运行一些 JavaScript 代码

```html
<div id="app">
  <button v-on:click="count += 1">+1</button>
  <span>{{ count }}</span>
</div>
<script>
  var vm = new Vue({
    el: 'app',
    data: {
      count: 0
    }
  })
</script>
```

### 事件处理方法

大多时候, 我们的事件处理逻辑都是复杂的, 这时候直接写在 `v-on` 指令中是不可行的, 因此 `v-on` 还可以接受一个需要调用的方法名称

```html
<div id="app">
  <button v-on:click="add">+1</button>
  <span>{{ count }}</span>
</div>
<script>
  var vm = new Vue({
    el: 'app',
    data: {
      count: 0
    },
    methods: {
      add() {
        // this 指向当前 vue 实例
        this.count += 1
      }
    }
  })
  vm.add() // 也可以直接用实例调用此方法
</script>
```

### 内联处理器中的方法

除了在 `v-on` 指令后面绑定一个方法, 我们也可以在内联的 JavaScript 语句中调用方法  
有时候我们需要在方法中访问原生的 DOM 事件, 可以用特殊的变量 `$event` 把它传入方法

```html
<div id="app">
  <button v-on:click="log('Hi')">+1</button>
</div>
<script>
  var vm = new Vue({
    el: 'app',
    methods: {
      log(msg) {
        // this 指向当前 vue 实例
        console.log(msg)
      }
    }
  })
</script>
```

## 事件修饰符

在日常开发场景中, `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求  
为了解决此类场景, Vue 为 `v-on` 提供了**事件修饰符**

> 修饰符是由**点**开头的指令后缀表示

- `.stop` 阻止事件继续传播
- `.prevent` 提交事件不再重载页面
- `.captrue` 使用事件捕获模式
- `.self` 只在 event.target 是当前元素资深时触发处理函数
- `.once` 点击事件将只会触发一次
- `.passive` 滚动事件的默认行为将会立即触发, 不会等待 `onScroll` 完成

## 按键修饰符

在监听键盘事件时, 我们经常要检查详细的按键, Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符

- `.enter` 回车键
- `.tab` tab 键
- `.delete` 删除和退格键
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input type="text" v-on:keyup.enter="submit" />
```

> Vue 允许我们将 `keyboardEvent.key` 暴露的任意有效按键名转换为 kebab-case 来作为修饰符
> 还可以通过全局 `config.keyCodes` 对象自定义按键修饰符别名 Vue.config.keyCodes.f1 = 112

## 系统修饰符

可以用以下修饰符来实现仅在按下相应按键时才出发鼠标或键盘事件的监听器

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

```html
<!-- alt + c -->
<input @keyup.alt.67="clear" />
```

## `.exact` 修饰符

`.exact`修饰符允许控制由精确的系统修饰符组合触发的事件

```html
<!-- 即使 alt 或 shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">a</button>

<!-- 有且只有 ctrl 被按下时才触发 -->
<button @click.ctrl.exact="onClick">a</button>
```

## 鼠标按钮修饰符

- `.left`
- `.right`
- `.middle`
