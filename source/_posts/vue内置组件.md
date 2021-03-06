---
title: vue内置组件
date: 2020-02-27 11:16:52
tags:
  - Vue
categories:
  - Vue
---

## component

- props:
  - `is`: string | ComponentDefinition | ComponentConstructor
  - `inline-template`: boolean
- 用法:
  渲染一个元组件为动态组件. 依 `is` 的值, 来决定那个组件被渲染

  ```HTML
  <!-- 动态组件由 vm 实例的属性值 componentId 控制 -->
  <component :is='componentId'></component>

  <!-- 也能够渲染注册过的组件或 prop 传入的组件 -->
  <component :is='$options.components.child'></component>
  ```

## transition

- Props:

  - `name`: string, 用于自动生成 CSS 过渡类名. 例如 `name: 'fade'` 将自动扩展为 `.fade-enter`, `.fade-enter-active`等
  - `appear`: boolean, 是否在初始渲染时使用过渡 默认 `false`
  - `css`: boolean, 是否使用 CSS 过渡类. 默认为 `true` 如果设置为 `false` 将只通过组件事件 触发注册的 JavaScript 钩子
  - `type`: string, 指定过渡事件类型, 侦听过渡何时结束. 有效值为 `transition` 和 `animation`. 默认 Vue 将自动检测出赤足时间长的为过渡事件类型.
  - `mode`: string, 控制离开/进入过渡的时间序列. 有效的模式有 `out-in` 和 `in-out` 默认同时进行
  - `duration`: number| { enter: number, leave: number } 指定过渡的持续时间. 默认情况下会等待过渡所在根元素的第一个 `transition` 或 `animation` 事件
  - `enter-class`: string
  - `leave-class`: string
  - `appear-class`: string
  - `enter-to-class`: string
  - `leave-to0class`: string
  - `appear-to0class`: string
  - `enter-active-class`: string
  - `leave-active-class`: string
  - `appear-active-class`: string

- 事件:

  - `before-enter`
  - `before-leave`
  - `before-appear`
  - `enter`
  - `leave`
  - `appear`
  - `after-enter`
  - `after-leave`
  - `after-appear`
  - `enter-cancelled`
  - `leave-cancelled`
  - `appear-cancelled`

- 用法:

  ```html
  <!-- 简单元素 -->
  <transition>
    <div v-if="show">SHOW</div>
  </transition>

  <!-- 动态组件 -->
  <transition name="fade" apear>
    <component :is="view"></component>
  </transition>

  <!-- 事件钩子 -->
  <div id="transition-demo">
    <transition @after-enter="transitionComplete"></transition>
  </div>
  ```

  ```js
  new Vue({
    methods: {
      transitionComponlete(el) {
        // 传入 'el' 这个 DOM 元素作为参数。
      }
    }
  }).$mount('#transition-demo')
  ```

## transition-group

- Props:
  - `tag`: string 默认为 `span`
  - `move-class`: 覆盖移动过渡期间应用的 CSS 类
  - 除了 `mode`, 其他特性和 `<transition>` 相同
- 事件:
  - 事件和`<transition>`相同
- 用法:
  `<transition-group>` 元素作为多个元素/组件的过渡效果. `<transition-group>` 渲染一个真实的 DOM 元素. 默认渲染 `<span>` 可以通过 `tag` 属性配置那个元素应该被渲染

  注意, 每个 `<transition-group>` 的子节点必须有独立的 key. 动画才能正常工作

  `<transition-group>` 支持通过 CSS transform 过渡移动. 当一个子节点被更新, 从屏幕上的位置发生变化. 他会被应用一个移动中的 CSS 类. 如果 CSS `transform` 属性是可以过渡属性, 当应用移动类时, 将会使用 FLIP 技术使元素流畅的到达动画终点

  ```html
  <transition-group tag="ul" name="slide">
    <li v-for="item in items" :key="item.id">
      {{ item.text }}
    </li>
  </transition-group>
  ```

## keep-alive

- Props:
  - `include`: 字符串或正则表达式, 只有名称匹配的组件会被缓存
  - `exclude`: 字符串或正则表达式, 任何名称匹配的组件都不会被缓存
  - `max`: 数字, 最多可以缓存多少组件实例
- 用法:
  `<keep-alive>` 包裹动态组件时, 它缓存不活动的组件实例. 而不是销毁他们. 和 `<transition>`相似.
  `<keep-alive>` 是一个抽象组件: 它自身不会渲染一个 DOM 元素. 也不会出现在组件的父组件链中

  当组件在 `<keep-alive>` 内被切换, 它的 `activated` 和 `deactivated` 这两个生命周期钩子函数将会被对应执行

  ```html
  <!-- 基本 -->
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>

  <!-- 多个条件判断的子组件 -->
  <keep-alive>
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
  </keep-alive>

  <!-- 和 `<transition>` 一起使用 -->
  <transition>
    <keep-alive>
      <component :is="view"></component>
    </keep-alive>
  </transition>
  ```

  注意，`<keep-alive>` 是用在其一个直属的子组件被开关的情形。如果你在其中有 v-for 则不会工作。如果有上述的多个条件性的子元素，`<keep-alive>` 要求同时只有一个子元素被渲染。

- `include` and `exclude`
  `include` 和 `exclude` 属性允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示：

  ```html
  <!-- 逗号分隔字符串 -->
  <keep-alive include="a,b">
    <component :is="view"></component>
  </keep-alive>

  <!-- 正则表达式 (使用 `v-bind`) -->
  <keep-alive :include="/a|b/">
    <component :is="view"></component>
  </keep-alive>

  <!-- 数组 (使用 `v-bind`) -->
  <keep-alive :include="['a', 'b']">
    <component :is="view"></component>
  </keep-alive>
  ```

  匹配首先检查组件自身的 name 选项，如果 name 选项不可用，则匹配它的局部注册名称 (父组件 components 选项的键值)。匿名组件不能被匹配。

- max
  最多可以缓存多少组件实例。一旦这个数字达到了，在新实例被创建之前，已缓存组件中最久没有被访问的实例会被销毁掉。

  ```html
  <keep-alive :max="10">
    <component :is="view"></component>
  </keep-alive>
  ```

## slot

- Props:
  - `name`: string, 用于命名插槽
- Usage:
  `<slot>` 元素作为组件模板之中的内容分发插槽. `<slot>`元素自身将被替换
