---
title: vue-router构建选项
date: 2020-03-11 14:51:56
tags:
  - VueRoute
categories:
  - VueRoute
---

## routes

- 类型: Array<RouteConfig>
  RouteConfig 的类型定义:

  ```ts
  interface RouteConfig = {
    path: string,
    component?: Component,
    name?: string, // 命名路由
    components?: { [name: string]: Component }, // 命名视图组件
    redirect?: string | Location | Function,
    props?: boolean | Object | Function,
    alias?: string | Array<string>,
    children?: Array<RouteConfig>, // 嵌套路由
    beforeEnter?: (to: Route, from: Route, next: Function) => void,
    meta?: any,

    // 2.6.0+
    caseSensitive?: boolean, // 匹配规则是否大小写敏感？(默认值：false)
    pathToRegexpOptions?: Object // 编译正则的选项
  }
  ```

## mode

- 类型: string
- 默认值: hash | abstract
- 可选值: hash | history | abstract
  配置路由模式:
  - hash: 使用 URL hash 值来作路由. 支持所有浏览器, 包括不支持 HTML5 History Api 的浏览器
  - history: 依赖 HTML5 History API 和服务器配置
  - abstract: 支持所有 JavaScript 运行环境, 如 Node.js 服务器端, 如果发现没有浏览器的 API, 路由会自动强制进入这个模式

## base

- 类型: string
- 默认值: '/'
  应用的基路径, 例如, 如果整个单页应用服务在 `/app/` 下, 然后 base 就应该设为 `/app/`

## linkActiveClass

- 类型: string
- 默认值: router-link-active
  全局配置 `<router-link>` 默认的激活 class

## linkExactActiveClass

- 类型: string
- 默认值: router-link-exact-active
  全局配置 `<router-link>` 默认的精确激活的 class

## scrollBehavior

- 类型: Function
  签名:

  ```
  type PositionDescriptor =
  { x: number, y: number } |
  { selector: string } |
  ?{}

  type scrollBehaviorHandler = (
  to: Route,
  from: Route,
  savedPosition?: { x: number, y: number }
  ) => PositionDescriptor | Promise<PositionDescriptor>

  ```

## parseQuery / stringifyQuery

- 类型: Function
  提供自定义查询字符串的解析/反解析函数

## fallback

- 类型: boolean
  当浏览器不支持 `history.pushState` 控制路由是否应该回退到 hash 模式. 默认为 `true`
