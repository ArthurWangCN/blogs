---
title: 小程序填坑指南
date: 2017-07-12 22:23:31
tags: 小程序
categories: 小程序
---
# 逻辑层
## 注册程序
`App()` 函数用来注册一个小程序。接受一个 `object` 参数，其指定小程序的生命周期函数等。

### 前台、后台定义
onShow：当小程序启动，或从后台进入前台显示，会触发 onShow 
onHide：当小程序从前台进入后台，会触发 onHide

当用户点击左上角关闭，或者按了设备 Home 键离开微信，小程序并没有直接销毁，而是 **进入了后台**；当再次进入微信或再次打开小程序，又会从后台进入前台。

需要注意的是：只有当小程序进入后台一定时间（目前为5分钟），或者系统资源占用过高，才会被真正的销毁。

当用户从扫一扫、转发等入口(场景值为1007, 1008, 1011, 1025)进入小程序，且没有置顶小程序的情况下退出，小程序会被销毁。

### 获取小程序实例
getApp()：我们提供了全局的 `getApp()` 函数，可以获取到小程序实例。

```
// other.js
var appInstance = getApp()
console.log(appInstance.globalData) // I am global data
```

+ `App()` 必须在 `app.js` 中注册，且不能注册多个。
+ 不要在定义于 `App()` 内的函数中调用 `getApp()` ，使用 `this` 就可以拿到 `app` 实例。
+ 不要在 `onLaunch` 的时候调用 `getCurrentPages()`，此时 `page` 还没有生成。
+ 通过 `getApp()` 获取实例之后，不要私自调用生命周期函数。


## 场景值
由于Android系统限制，目前还无法获取到按 `Home` 键退出到桌面，然后从桌面再次进小程序的场景值，对于这种情况，会保留上一次的场景值。


## 注册页面
### setData
接受一个对象，以 `key，value` 的形式表示将 `this.data` 中的 `key` 对应的值改变成 `value`。


+ 直接修改 this.data 而不调用 `this.setData` 是无法改变页面的状态的，还会造成数据不一致
+ 单次设置的数据不能超过1024kB，请尽量避免一次设置过多的数据。


## 页面路由
### 页面栈
#### getCurrentPages
`getCurrentPages()` 函数用于获取当前页面栈的实例，以数组形式按栈的顺序给出，第一个元素为首页，最后一个元素为当前页面。

不要尝试修改页面栈，会导致路由以及页面状态错误


### 路由方式
+ **navigateTo, redirectTo 只能打开非 tabBar 页面。**
+ switchTab 只能打开 tabBar 页面。
+ reLaunch 可以打开任意页面。
+ 页面底部的 tabBar 由页面决定，即只要是定义为 tabBar 的页面，底部都有 tabBar。
+ 调用页面路由带的参数可以在目标页面的onLoad中获取。


## 模块化
我们可以将一些公共的代码抽离成为一个单独的 `js` 文件，作为一个模块。模块只有通过 `module.exports` 或者 `exports` 才能对外暴露接口。

+ `exports` 是 `module.exports` 的一个引用，因此在模块里边随意更改 `exports` 的指向会造成未知的错误。所以我们更推荐开发者采用 `module.exports` 来暴露模块接口，除非你已经清晰知道这两者的关系。
+ 小程序目前不支持直接引入 `node_modules` , 开发者需要使用到 `node_modules` 时候建议拷贝出相关的代码到小程序的目录中。


在需要使用这些模块的文件中，使用 `require(path)` 将公共代码引入

+ `require` 暂时不支持绝对路径



# 视图层
## WXML
### 数据绑定
#### 关键字
**需要在双引号之内**

+ true：boolean 类型的 true，代表真值。
+ false： boolean 类型的 false，代表假值。

```
<checkbox checked="{{false}}"> </checkbox>
```

**特别注意：不要直接写 `checked="false"`，其计算结果是一个字符串，转成 `boolean` 类型后代表真值。**

#### 对象
**如有存在变量名相同的情况，后边的会覆盖前面，如：**

```
<template is="objectCombine" data="{{...obj1, ...obj2, a, c: 6}}"></template>
```

```
Page({
  data: {
    obj1: {
      a: 1,
      b: 2
    },
    obj2: {
      b: 3,
      c: 4
    },
    a: 5
  }
})
```

最终组合成的对象是 `{a: 5, b: 3, c: 6}`。


**花括号和引号之间如果有空格，将最终被解析成为字符串**

```
<view wx:for="{{[1,2,3]}} ">
  {{item}}
</view>
```

等同于

```
<view wx:for="{{[1,2,3] + ' '}}">
  {{item}}
</view>
```


### 列表渲染
**wx:for**
在组件上使用 `wx:for` 控制属性绑定一个数组，即可使用数组中各项的数据重复渲染该组件。

默认数组的当前项的下标变量名默认为 `index`，数组当前项的变量名默认为 `item`


### 引用
#### 作用域
`import` 有作用域的概念，即只会 import 目标文件中定义的 template，而不会 import 目标文件 import 的 template。

**如：C import B，B import A，在C中可以使用B定义的template，在B中可以使用A定义的template，但是C不能使用A定义的template。**

