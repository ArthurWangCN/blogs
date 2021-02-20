---
title: Vue Router(一)：起步
date: 2020-04-26 17:52:59
tags: Vue
categories: [前端, Vue]
---

>我们需要做的是，将组件 (components) 映射到路由 (routes)，然后告诉 Vue Router 在哪里渲染它们

## 创建路由的步骤
1. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)；
2. 定义 (路由) 组件，或者从其他地方import；
3. 定义路由——每个路由应该映射一个组件；
4. 创建 router 实例，然后传 `routes` 配置；
5. 创建和挂载根实例——通过router注入路由。


+ 通过 `this.$router` 访问路由器
+ 通过 `this.$route` 访问当前路由
+ `this.$router` 和 `router` 使用起来完全一样
+ `<router-link>` 对应的路由匹配成功，将自动设置 class 属性值 `.router-link-active`

## 动态路由
>需要把某种模式匹配到的所有路由，全都映射到同个组件。vue-router 的路由路径中使用“动态路径参数”(dynamic segment) 来达到这个效果

+ 动态路径参数以冒号开头—— `{ path: '/user/:id', component: User }`；
+ 参数值会被设置到 `this.$route.params`；
+ 多段路径—— `/user/:username/post/:post_id`；
+ 还有 `$route.query` (如果 URL 中有查询参数) 和 `$route.hash` ；

### 响应路由参数变化
>路由参数变化，原来的组件实例会被复用，这也意味着组件的生命周期钩子不会再被调用

**解决**：
1、可以简单地 watch (监测变化) `$route` 对象：

```javascript
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
```

2、beforeRouteUpdate 导航守卫

```javascript
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // 响应路由变化...
    // 记得调用 next()
  }
}
```

### 捕获所有路由
+ 常规参数只会匹配被 `/` 分隔的 URL 片段中的字符；
+ 匹配任意路径，使用通配符 (`*`)；
+ 路由 `{ path: '*' }` 通常用于客户端 404 错误；
+ `path: '/user-*'` 会匹配以 `/user-` 开头的任意路径
+ 当使用一个通配符时，`$route.params` 内会自动添加一个名为 `pathMatch` 参数；
+ pathMatch 参数包含了 URL 通过通配符被匹配的部分；

### 匹配优先级
+ 同一个路径可以匹配多个路由；
+ 匹配的优先级就按照路由的定义顺序：谁先定义的，谁的优先级就最高。

## 嵌套路由
+ 要在嵌套的出口中渲染组件，需要在 VueRouter 的参数中使用 `children` 配置；
+ 以 `/` 开头的嵌套路径会被当作根路径；
+ 访问 `/user/foo` 时，`User` 是不会渲染任何东西，可提供一个空的子路由——`{ path: '', component: xxx }` 。