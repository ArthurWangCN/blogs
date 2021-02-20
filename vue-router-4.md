---
title: Vue Router(四)：路由进阶
date: 2020-04-26 19:28:53
tags: Vue
categories: [前端, Vue]
---

## 导航守卫
>“导航”表示路由正在发生改变。
>导航守卫可以看做是 vue-router 的生命周期钩子

+ vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航
+ 植入路由导航的机会：全局的, 单个路由独享的, 或者组件级的
+ 参数或查询的改变并不会触发进入/离开的导航守卫（通过观察 $route 对象来应对这些变化，或使用 beforeRouteUpdate 的组件内守卫通过观察 ）

### 全局前置守卫
使用 `router.beforeEach` 注册一个全局前置守卫：

```javascript
const router = new VueRouter({ ... })
router.beforeEach((to, from, next) => {
  // ...
})
```
当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 等待中。

next：
+ `next()`: 进行管道中的下一个钩子。
+ `next(false)`: 中断当前的导航
+ `next('/')` 或者 `next({ path: '/' })`: 跳转到一个不同的地址
+ next(error): 一个 Error 实例，导航会被终止且该错误会被传递给 router.onError() 注册过的回调。

**确保要调用 next 方法，否则钩子就不会被 resolved**。


### 全局解析守卫
router.beforeResolve —— 在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。


### 全局后置钩子

```javascript
router.afterEach((to, from) => {
  // ...
})
```
不会接受 next 函数也不会改变导航本身


### 路由独享的守卫
beforeEnter：

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```


### 组件内的守卫
+ beforeRouteEnter
+ beforeRouteUpdate (2.2 新增)
+ beforeRouteLeave

```javascript
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
    // 可以通过传一个回调给 next来访问组件实例
    // 导航被确认的时候执行回调，并且把组件实例作为回调方法的参数
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

### 完整的导航解析流程
1. 导航被触发。
2. 在失活的组件里调用离开守卫。
3. 调用全局的 beforeEach 守卫。
4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5. 在路由配置里调用 beforeEnter。
6. 解析异步路由组件。
7. 在被激活的组件里调用 beforeRouteEnter。
8. 调用全局的 beforeResolve 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 afterEach 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。


## 路由元信息
定义路由的时候可以配置 `meta` 字段：

```javascript
{
	path: 'bar',
	component: Bar,
	// a meta field
	meta: { requiresAuth: true }
}
```
遍历 `$route.matched` 来检查路由记录中的 meta 字段：

```javascript
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```

## 过渡动效

```javascript
<transition>
  <router-view></router-view>
</transition>
```


## 数据获取
有时候，进入某个路由后，需要从服务器获取数据。例如，在渲染用户信息时，你需要从服务器获取用户的数据。我们可以通过两种方式来实现：

1. **导航完成之后获取**：先完成导航，然后在接下来的组件生命周期钩子(created)中获取数据。在数据获取期间显示“加载中”之类的指示。
2. **导航完成之前获取**：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。	`beforeRouteEnter` 中获取数据，获取后执行 `next()`



## 滚动行为
注意: 这个功能只在支持 `history.pushState` 的浏览器中可用。

```javascript
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
    // 第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。
  }
})
```

对于所有路由导航，简单地让页面滚动到顶部:

```javascript
scrollBehavior (to, from, savedPosition) {
  return { x: 0, y: 0 }
}
```


## 路由懒加载
>当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

首先，可以将异步组件定义为返回一个 Promise 的工厂函数：

```javascript
const Foo = () => Promise.resolve({ /* 组件定义对象 */ })
```

第二，在 Webpack 2 中，我们可以使用动态 import语法来定义代码分块点 (split point)：

```javascript
import('./Foo.vue') // 返回 Promise
```
如果使用babel，需要 `syntax-dynamic-import` 插件来正确解析语法。

结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。

```javascript
const Foo = () => import('./Foo.vue')
```

在路由配置中什么都不需要改变，只需要像往常一样使用 Foo：

```javascript
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```


**把组件按组分块：**

有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 命名 chunk，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。

```javascript
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

Webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。