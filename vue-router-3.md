---
title: Vue Router(三)：命名视图、重定向和组件传参
date: 2020-04-26 19:27:59
tags: Vue
categories: [前端, Vue]
---

## 命名视图
>同时 (同级) 展示多个视图，而不是嵌套展示

如果 router-view 没有设置名字，那么默认为 `default`
```javascript
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```

```javascript
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
```

## 重定向

```javascript
{ path: '/a', redirect: '/b' }
// 或对象形式
{ path: '/a', redirect: { name: 'foo' }}
// 甚至是一个方法，动态返回重定向目标
{ path: '/a', redirect: to => {
	// 方法接收 目标路由 作为参数
	// return 重定向的 字符串路径/路径对象
}}
```

注意：导航守卫并没有应用在跳转路由上，而仅仅应用在其目标上。

## 别名
>/a 的别名是 /b，意味着，当用户访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a，就像用户访问 /a 一样。

```javascript
{ path: '/a', component: A, alias: '/b' }
```

“别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。


## 路由组件传参
使用 props 将组件和路由解耦：

```javascript
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```
+ 如果 props 被设置为 true，`route.params` 将会被设置为组件属性
+ 如果 props 是一个对象，它会被按原样设置为组件属性。当 props 是静态的时候有用。
+ 可以创建一个函数返回 props。这样你便可以将参数转换成另一种类型，将静态值与基于路由的值结合