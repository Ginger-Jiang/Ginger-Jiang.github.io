---
title: vue-router动态路由匹配
date: 2020-03-03 20:53:22
tags:
  - VueRoute
categories:
  - VueRoute
---

## 动态路由匹配

我们经常需要把某种模式匹配到的所有路由, 全部映射到同个组件.
如: 我们有个 User 组件, 对于所有 ID 各不相同的用户, 都要使用这个组件来渲染, 那么,我们可以在 vue-router 的路由路径中使用 动态路径参数来达到这个效果:

```js
const User = {
  template: '<div>User</div>'
}
const routed = [
  // 动态路径参数以冒号开头
  {
    path: '/user/:id',
    component: User
  }
]
```

现在, 像 `/user/foo` 和 `/user/bar` 都将映射到相同的路由

一个"路径参数"使用冒号`:`标记, 当匹配到一个路由时, 参数值会被设置到 `this.$route.params`. 可以在每个组件内使用.

```js
// 输出用户id
const User = {
  template: '<div>{{ this.$route.params.id }}</div'
}
```

可以在一个路由中设置多段"路径参数", 对应的值都会设置到 `$route.params`中

- `/user/:username` 匹配: `/user/ginger` params: `{username: "ginger"}`
- `/user/:username/post/:postid` 匹配: `/user/ginger/post/123` params: `{username: "ginger", postid: 123}`

## 响应路由参数的变化

> 从 `/user/foo`导航到 `/user/bar`，原来的组件实例会被复用, 这意味着组件的生命周期钩子不会再被调用

复用组件事, 想对路由参数的变化做出响应的话, 可以简单的 `watch` `$route` 对象

```js
const user = {
  template: '...',
  watch: {
    '$route': (to, from) {
      // 对路由变化做出响应
    }
  }
}
```

或者使用 `beforeRouteUpdate` 导航守卫

```js
const User = {
  template: '...'
  beforeRouteUpdate(to , from. next) {
    // react to route changes...
    // don't forget to call next()
  }
}
```

## 捕获所有路由或 404 Not found 路由

常规参数智慧匹配被 `/` 分隔的 URL 片段中的字符, 如果想匹配任意路径, 我们可以使用通配符 (`*`)

```js
{
  // 匹配所有路径
  path: '*'
}
{
  // 匹配user开头的所有路径
  path: '/user-*'
}
```

当使用通配符时吗请确保路由的顺序是正确的, 也就是说含有通配符的路由应该放在最后, 路由(path: `*`)通常用于客户端 404 错误, 如果你使用了 History 模式, 需要配置服务器支持

当使用一个通配符时, `$route.params` 内会自动添加一个名为 `pathMatch` 参数, 它包含了 URl 通过通配符被匹配的部分

```js
// 给出一个路由 ( path: 'user-*')
this.$route.push('user-admin')
this.$route.params.pathMatch // admin

// 给出一个路由 { path: '*' }
this.$route.push('/non-existing')
this.$route.params.pathMatch // '/non-existing'
```

## 高级匹配模式

`vue-router` 使用 `path-to-regexp` 作为路径匹配引擎, 所以支持很多高级的匹配模式

## 匹配优先级

路由匹配优先级按照路由的定义顺序, 谁先定义的, 谁的优先即就最高
