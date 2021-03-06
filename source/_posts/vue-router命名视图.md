---
title: vue-router命名视图
date: 2020-03-04 11:24:14
tags:
  - VueRoute
categories:
  - VueRoute
---

## 命名视图

有时候, 我们想要在一个页面内, 同时展示多个视图. 而不是嵌套展示, 可以使用命名视图.
如果 `<router-view>` 没有设置名字, name 默认为 `default`

```html
<router-view class="one"></router-view>
<router-view class="two" name="a"></router-view>
<router-view class="shree" name="b"></router-view>
```

一个视图使用一个组件渲染, 因此对于同个路由, 多个视图就需要多个组件, 确保正确使用 `components` 配置

```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

## 嵌套命名视图

我们也有可能使用命名视图创建嵌套视图的复杂布局。这时你也需要命名用到的嵌套 router-view 组件

```html
<!-- UserSettings.vue -->
<div>
  <h1>User Settings</h1>
  <NavBar />
  <router-view />
  <router-view name="helper" />
</div>
```

```js
{
  path: '/settings',
  // 你也可以在顶级路由就配置命名视图
  component: UserSettings,
  children: [{
    path: 'emails',
    component: UserEmailsSubscriptions
  }, {
    path: 'profile',
    components: {
      default: UserProfile,
      helper: UserProfilePreview
    }
  }]
}
```
