---
title: 微信小程序全方位深度解析 第一讲
date: 2017-07-11 14:04:45
tags: 小程序
categories: 小程序
---
微信小程序自从提出以来就在IT界掀起了一阵大浪，前些天去寄快递的时候发现某通快递公司填写快递信息就是用的小程序。有些人说小程序被过分吹捧了，但不可否认的是现在小程序已经进入到了我们的日常生活。


## 小程序环境搭建
微信小程序 [官方下载地址](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/download.html) 。

**流程：**

1. 根据系统下载对应安装软件
2. 下载 `node.js` ,安装
3. 安装微信开发者工具
4. 登陆（微信号需要成为微信开发者）-> [如何成为微信开发者](http://kf.qq.com/faq/120911VrYVrA1307306biMFz.html)
5. 选择 `无AppID`（没有装nodejs的话没有这个选项）


## 目录结构及配置
创建完一个小程序后结构如下：

![小程序结构](http://blogpic.at15cm.com/minip-1.pn)

+ .js后缀的文件是脚本文件，页面的交互等代码在这里实现；
+ .json后缀的文件是配置文件，主要是json数据格式存放，用于设置程序的配置效果；
+ .wxss后缀的是样式表文件，类似于前端中的css,用于对界面进行美化；
+ .wxml后缀的文件是页面结构文件，用于构建页面，在页面上增加控件

**app.js** 是小程序的脚本代码。我们可以在这个文件中监听并处理小程序的生命周期函数、声明全局变量。调用框架提供的丰富的 API，如本例的同步存储及同步读取本地数据。

```
//app.js
App({
  //程序初始化的时候执行onLaunch里面的内容
  onLaunch: function() {
    //调用API从本地缓存中获取数据
    var logs = wx.getStorageSync('logs') || []
    logs.unshift(Date.now())
    wx.setStorageSync('logs', logs)
  },
  //全局的方法
  getUserInfo: function(cb) {
    var that = this
    if (this.globalData.userInfo) {
      typeof cb == "function" && cb(this.globalData.userInfo)
    } else {
      //调用登录接口
      wx.getUserInfo({
        withCredentials: false,
        success: function(res) {
          that.globalData.userInfo = res.userInfo
          typeof cb == "function" && cb(that.globalData.userInfo)
        }
      })
    }
  },
//全局的属性
  globalData: {
    userInfo: null
  }
})
```

**app.json** 是对整个小程序的全局配置。我们可以在这个文件中配置小程序是由哪些页面组成，配置小程序的窗口背景色，配置导航条样式，配置默认标题。注意该文件不可添加任何注释。

```
{
  "pages":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "WeChat",
    "navigationBarTextStyle":"black"
  }
}
```

**app.wxss** 是整个小程序的公共样式表。我们可以在页面组件的 class 属性上直接使用 app.wxss 中声明的样式规则。

```
/**app.wxss**/
.container {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  padding: 200rpx 0;
  box-sizing: border-box;
} 
```


