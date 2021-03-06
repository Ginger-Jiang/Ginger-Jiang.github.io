---
title: vue-router命名路由
date: 2020-03-04 11:06:50
tags:
  - VueRoute
categories:
  - VueRoute
---

## 命名路由

有时候, 通过一个名称来标识一个路由显得更方便一些, 特别是在链接一个路由, 或者是执行一些跳转的时候
可以在创建 Router 实例的时候, 在 `routes` 配置中给某个路由设置名称

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userid',
      name: 'user',
      component: User
    }
  ]
})
```

要链接到一个命名路由, 可以给 `router-link` 的 `to` 属性传一个对象

```html
<!-- 声明式 -->
<router-link :to="{name: 'user', params: {userId: 123}}"> </router-link>
```

```js
// 编程式
router.push({
  name: 'user',
  params: {
    userId: 123
  }
})
```
