---
title: Vue Router(二)：编程式导航
date: 2020-04-26 18:05:30
tags: Vue
categories: [前端, Vue]
---

`router-link` 实质上是创建 a 标签，我们也可以通过 router 的实例方法来实现。

## router.push
>router.push(location, onComplete?, onAbort?)

当你点击 `<router-link>` 时，这个方法会在内部调用，所以说，点击 `<router-link :to="...">` 等同于调用 `router.push(...)`。

如果提供了 path ，params会被忽略：

```javascript
// 字符串
router.push('home')
// 对象
router.push({ path: 'home' })
// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})
// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

需要提供路由的 name 或手写完整的带有参数的 path：

```javascript
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

+ onComplete —— 会在导航成功完成 (在所有的异步钩子被解析之后) 的时候进行相应的调用
+ onAbort —— 会在终止 (导航到相同的路由、或在当前导航完成之前导航到另一个不同的路由) 的时候进行相应的调用

如果目的地和当前路由相同，只有参数发生了改变，需要使用 `beforeRouteUpdate` 来响应这个变化 (比如抓取用户信息)


## router.replace
跟 router.push 唯一的不同就是，它不会向 history 添加新记录，而是替换掉当前的 history 记录。


## router.go(n)
这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`