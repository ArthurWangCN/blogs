---
title: vue中router与route
date: 2020-02-10 23:28:27
tags: [Vue, 前端]
categories: 前端
---
## router

`$router` 对象是全局路由的实例，是 router 构造方法的实例，主要是实现路由跳转。

路由实例方法：

**一、 push**

1、字符串 `this.router.push(′home′)`
2、对象 `this.router.push({ path: 'home' })`
3、命名的路由 `this.router.push(name:′user′,params:userId:123)`
4、带查询参数 `/register?plan=123`，变成 `this. router.push({name:'user',params:{userId:123}})`


push 方法其实和 `<router-link :to="…">` 是等同的。

注意：push 方法的跳转会向 history 栈添加一个新的记录，当我们点击浏览器的返回按钮时可以看到之前的页面。


**二、go**

页面路由跳转

前进或者后退 `this.$router.go(-1)` // 后退


**三、replace**

push 方法会向 history 栈添加一个新的记录，而replace方法是替换当前的页面，不会向 history 栈添加一个新的记录.

```
// 一般使用replace来做404页面
this.$router.replace('/')
```

配置路由时path有时候会加 `/` 有时候不加,以 `/` 开头的会被当作根路径，就不会一直嵌套之前的路径。



## route

`$route` 是一个跳转的路由对象，route对象表示当前的路由信息，包含了当前 URL 解析得到的信息，每一个路由都会有一个 route 对象，是一个局部的对象，可以获取对应的 name,path,params,query 等。

`$route` 是不可变的，每次成功的导航后都会产生一个新的对象.

1. `$route.path` 字符串，对应当前路由的路径，总是解析为绝对路径，如"/foo/bar"。
2.  `$route.params`一个 key/value 对象，包含了 动态片段 和 全匹配片段， 如果没有路由参数，就是一个空对象。
3. `$route.query` 一个 key/value 对象，表示 URL 查询参数。。
4. `$route.hash` 当前路由的hash值 (不带#) ，如果没有 hash 值，则为空字符串。锚点*
5. `$route.fullPath` 完成解析后的 URL，包含查询参数和hash的完整路径。
6. `$route.matched` 数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象。
7. `$route.name` 当前路径名字
8. `$route.meta` 路由元信息


本文来自 [「繁华落烬，戏命如诗」](https://blog.csdn.net/liuyifeng0000/article/details/104032928/)