---
title: vue特殊特性
date: 2020-02-27 09:07:18
tags:
  - Vue
categories:
  - Vue
---

## key

- 预期: number | string
  `key` 的投入数属性主要用在 Vue 的虚拟 DOM 算法. 在新旧 nodes 对比时辨识 VNode. 如果不使用 key, Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法. 而使用 key 时, 他会基于 key 的变化重新排列元素顺序. 并且会移除 key 不存在的元素

  有相同父元素的子元素必须有独特的 key, 重复的 key 会造成渲染错误

  最常见的用例是结合 `v-for`

  ```html
  <ul>
    <li v-for="item in items" v-bind:key="item.id">...</li>
  </ul>
  ```

  它也可以用于强制特换元素/组件而不是重复使用它, 当你遇到如下场景时他可能会很有用

  - 完整的触发组件的生命周期钩子
  - 触发过渡

  例如:

  ```html
  <transition>
    <span :key="text">{{ text }}</span>
  </transition>
  ```

  当 `text` 发生改变时, `<span>` 总是会被替换而不是被修改. 因为会触发过渡

## ref

- 预期: string
  `ref` 被用来给元素或子组件注册引用信息. 引用信息将会被注册在父组件的 `$refs` 对象上. 如果在普通的 DOM 元素上使用. 引用指向的就是 DOM 元素. 如果用在子组件上, 引用就指向组件实例

  ```html
  <!-- `vm.$refs.p` will be the DOM node -->
  <p ref="p">hello</p>

  <!-- `vm.$refs.child` will be the child component instance -->

  <child-component ref="child"></child-component>
  ```

  当 v-for 用于元素或组件的时候，引用信息将是包含 DOM 节点或组件实例的数组。

  关于 ref 注册时间的重要说明：因为 ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问它们 - 它们还不存在！`$refs` 也不是响应式的，因此你不应该试图用它在模板中做数据绑定。

## is

- 预期: string | Object
  用于动态组件且基于 DOM 内模板的限制来工作

  示例:

  ```html
  <!-- 当 `currentView` 改变时，组件也跟着改变 -->
  <component v-bind:is="currentView"></component>

  <!-- 这样做是有必要的，因为 `<my-row>` 放在一个 -->
  <!-- `<table>` 内可能无效且被放置到外面 -->
  <table>
    <tr is="my-row"></tr>
  </table>
  ```
