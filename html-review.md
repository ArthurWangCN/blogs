---
title: HTML 回顾
date: 2020-02-12 14:48:08
tags: HTML
categories: [前端, HTML]
---

## doctype的意义是什么

+ 让浏览器以标准模式渲染（标准盒模型IE）
+ 让浏览器知道元素的合法性（HTML4、XHTML等）

## HTML XHTML HTML5的关系
+ HTML属于sGML
+ XHTML属于XML,是HTML进行XML严格化的结果
+ HTML5不属于SGML或XML,比XHTML宽松

## HTML5有什么变化
+ 新的语义化元素
+ 表单增强
+ 新的API（离线、音视频、图形、实时通信、本地存储、设备能力）
+ 分类和嵌套变更

## em和i有什么区别
+ em是语义化的标签,表强调
+ i是纯样式的标签,表斜体
+ HTML5中i不推荐使用,一般用作图标

## 语义化的意义是什么
+ 开发者容易理解
+ 机器容易理解结构(搜索、读屏软件)
+ 有助于SEo
+ semantic microdata

## 哪些元素可以自闭合
+ 表单元素 input
+ 图片img
+ br hr
+ meta link

## HTML和DOM的关系
+ HTML 是“死”的
+ DOM 由 HTML 解析而来,是活的
+ JS 可以维护 DOM

## property和 attribute的区别
+ attribute是“死”的
+ property是“活”的
+ 二者不相互影响

```
<input type="text" value="1" />
```

```
$0.value	//1
//对value进行修改
$0.value = 2	//输入框显示2
$0.getAttribute('value')	//1
//通过setAttribute修改
$0.setAttribute('value', 3)	//输入框仍是2，DOM结构为<input type="text" value="3" />
```

## form的作用有哪些
+ 直接提交表单
+ 使用 submit/ reset按钮
+ 便于浏览器保存表单
+ 第三方库可以整体提取值
+ 第三方库可以进行表单验证


