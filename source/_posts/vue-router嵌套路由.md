---
title: vue-router嵌套路由
date: 2020-03-04 10:47:57
tags:
  - VueRoute
categories:
  - VueRoute
---

## 嵌套路由

实际生活中的应用界面, 通常由多层嵌套的组件组合而成. URL 中各段动态路径也按某种结构对应嵌套的各层组件
借助 `vur-router` 使用嵌套路由配置, 可以简单的表达这种关系

```html
<div id="app">
  <router-view></router-view>
</div>
```

```js
const User = {
  template: '<div>{{ $route.params.id }}</div>'
}

const routes = [
  {
    path: '/user',
    component: User
  }
]

const router = new VueRouter({
  routes
})
```

这里的 `<router-view>` 是顶层的出口, 渲染最高级路由匹配到的组件.
一个被渲染的组件可以包含自己的嵌套 `<router-view>`

```js
// 给 User 组件加上嵌套的 <router-view>
const User = {
  template: `
  <div>
    <h2>User<h2>
    <router-view></router-view>
  </div>
  `
}
```

要在嵌套的出口中渲染组件, 需要在 `VurRouter` 的参数中使用 `children` 配置

```js
const router = new VueRouter({
  routes: [
    {
      path: 'user',
      component: User,
      chidren: [
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        },
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        }
      ]
    }
  ]
})
```

> 以 `/` 开头的嵌套路径会被当做根路径,

当你访问 `/user/foo` 时，User 的出口是不会渲染任何东西，这是因为没有匹配到合适的子路由。如果你想要渲染点什么，可以提供一个 空的 子路由：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      component: User,
      children: [
        // 当 /user/:id 匹配成功，
        // UserHome 会被渲染在 User 的 <router-view> 中
        { path: '', component: UserHome }

        // ...其他子路由
      ]
    }
  ]
})
```
