---
title: 小程序/小游戏生命周期
date: 2018-06-15 17:49:04
tags: 小程序
categories: 小程序
---
## wx.exitMiniProgram(Object object)
退出当前小游戏

### 参数
object

|属性	|类型	|默认值	|是否必填	|说明	|
|:---:|:---:|:---:|:---:|:---:|
|success	|function|		|否	|接口调用成功的回调函数|
|fail	|function|		|否	|接口调用失败的回调函数	|
|complete	|function|		|否	|接口调用结束的回调函数（调用成功、失败都会执行）|


<br /><br />

## wx.getLaunchOptionsSync()
返回小程序启动参数

### 返回值

**LaunchOption**

### 启动参数

| 属性 |  	类型| 	说明| 
| :---:| :---:| :---:| 
| scene| 	number	| 场景值	| 
| query	| Object	| 启动参数	| 
| isSticky| 	boolean	| 当前小游戏是否被显示在聊天顶部| 	
| shareTicket| 	string	| shareTicket| 


<br /><br />

## wx.onHide(function callback)
监听小游戏隐藏到后台事件。锁屏、按 HOME 键退到桌面、显示在聊天顶部等操作会触发此事件。

### 参数
callback

监听事件的回调函数


<br /><br />

## wx.offHide(function callback)
取消监听小游戏隐藏到后台事件。锁屏、按 HOME 键退到桌面、显示在聊天顶部等操作会触发此事件。

### 参数
callback

取消监听事件的回调函数


<br /><br />

## wx.onShow(function callback)
监听小游戏回到前台的事件

参数

callback,监听事件的回调函数

### callback 回调函数
参数 res

|属性|	类型|	说明|
|:---:|:---:|:---:|
|scene|	string|	场景值|	
|query|	Object|	查询参数|	
|shareTicket|	string|	shareTicket|


<br /><br />

## wx.offShow(function callback)
取消监听小游戏回到前台的事件

### 参数
callback

取消监听事件的回调函数


<br /><br />

本文来自 [微信公众平台](https://developers.weixin.qq.com/minigame/dev/document/system/life-cycle/wx.offShow.html)
