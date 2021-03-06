---
title: vue选项其他
date: 2020-02-24 22:50:32
tags:
  - Vue
categories:
  - Vue
---

## name

- 类型: `string`
- 限制: 只有作为组件选项时起作用
- 详细:
  允许组件模板递归的调用自身, 组件在全局用 `Vue.component()` 注册时, 全局 ID 自动作为组件的 name
  指定 `name` 选项的另一个好处是便于调试, 有名字的组件有更友好的警告信息

## delimiters

- 类型: `Array<string>`
- 默认值: `["{{","}}]`
- 限制: 这个选项只在完整构建版本中的浏览器内编译时可用
- 详细:
  改变纯文本插入分隔符
- 示例

  ```js
  new Vue({
    delimiters: `['${''}']`
  })

  // 分隔符变成了 ES6 模板字符串的风格
  ```

## functional

- 类型: `boolean`
- 详细:
  使组件无状态(没有`data`)和无示例(没有`this`上下文). 他们用一个简单的 `render` 函数返回虚拟节点使他们渲染的代价更小

## model

- 类型: `{ prop?: string, event?: string}`
- 详细:
  允许一个自定义组件在使用`v-model`时定制 prop 和 event. 默认情况下, 一个组件上的`v-model`会把`value`用作 prop 且把`input`用作 event, 但是一些输入类型比如单选框和复选框按钮可能想使用 `value` prop 来打到不同的目的
- 示例
  ```js
  Vue.component('my-checkbox', {
    model: {
      prop: 'checked',
      event: 'change'
    },
    props: {
      // this allows using the `value` prop for a different purpose
      value: String,
      // use `checked` as the prop which take the place of `value`
      checked: {
        type: Number,
        default: 0
      }
    }
    // ...
  })
  ```
  ```html
  <my-checkbox v-model="foo" value="some value"></my-checkbox>
  ```
  上述代码相当于:
  ```html
  <my-checkbox :checked="foo" @change="val => { foo = val }" value="some value">
  </my-checkbox>
  ```

## inheritAttrs

- 类型: `boolean`
- 默认值: `true`
- 详细:
  默认情况下父作用域的不被认作 props 的特性绑定 (attribute bindings) 将会“回退”且作为普通的 HTML 特性应用在子组件的根元素上。当撰写包裹一个目标元素或另一个组件的组件时，这可能不会总是符合预期行为。通过设置 inheritAttrs 到 false，这些默认行为将会被去掉。而通过 (同样是 2.4 新增的) 实例属性 \$attrs 可以让这些特性生效，且可以通过 v-bind 显性的绑定到非根元素上。

## comments

- 类型: `boolran`
- 默认值: `false`
- 限制: 这个选项只有在完成构建版本中的浏览器内编译时可用
- 详细:
  当设为 `true` 时, 将会保留且渲染模板中的 HTML 注释
