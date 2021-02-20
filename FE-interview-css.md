---
title: FE interview CSS 代码题
date: 2017-04-20 18:05:57
tags: [前端, 面试]
categories: 前端
---
## 如何居中一个浮动元素
父元素和子元素同时左浮动，然后父元素相对左移动50%，再然后子元素相对右移动50%，或者子元素相对左移动-50%也就可以了。

```
//html goes here...
<div class="box1">
	<h1 class="title">title goes here</h1>
</div>
```

```
//CSS goes here...
.box1{
	position:relative;
	left:50%;
	float:left;
}
.title{
	position:relative;
	float:left;
	right:50%;
}
```


## CSS实现水平垂直居中
负边距+定位：水平垂直居中（Negative Margin）
使用绝对定位将content的定点定位到body的中心，然后使用负margin（content宽高的一半），
将content的中心拉回到body的中心，已到达水平垂直居中的效果。

```
.align-center{
	position:absolute;
	left:50%;
	top:50%;
	width:400px;
	height:400px;
	margin-top:-200px;
	margin-left:-200px;
	border:1px dashed #333;
}
```


## css实现三栏布局，中间自适应

### 方法一：自身浮动法
左栏左浮动，右栏右浮动

```
.left , .right{
    height: 300px;
    width: 200px;
}
.right{
    float: right;
    background-color: red;
}
.left{
    float: left;
    background-color: #080808;
}
.middle{
    height: 300px;
    margin: 0 200px;//没有这行，当宽度缩小到一定程度的时候，中间的内容可能换行
    background-color: blue;
}
```

### 方法二：margin负值法
```
//html goes here...
<div class="middle"></div>
<div class="left"></div>
<div class="right"></div>
```


```
//CSS goes here...
body{
    margin: 0;
    padding: 0;
}
.left , .right{
    height: 300px;
    width: 200px;
    float: left;
}
.right{
    margin-left: -200px;
    background-color: red;
}
.left{
    margin-left: -100%;
    background-color: #080808;
}
.middle{
    height: 300px;
    width: 100%;
    float: left;
    background-color: blue;
}
```


### 方法三：绝对定位法
左右两栏采用绝对定位，分别固定于页面的左右两侧，中间的主体栏用左右margin值撑开距离

```
<div class="left">left</div>
<!--这种方法没有严格限定中间这栏放置何处-->
<div class="middle">middle</div>
<div class="right">right</div>
```

```
//CSS goes here...
body{
    margin: 0;
    padding: 0;
}
.left , .right{
    top: 0;
    height: 300px;
    width: 200px;
    position: absolute;
}
.right{
    right: 0;
    background-color: red;
}
.left{
    left: 0;
    background-color: #080808;
}
.middle{
    margin: 0 200px;
    height: 300px;
    background-color: blue;
}
```