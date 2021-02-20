---
title: Flex 布局教程：实例篇
date: 2017-05-08 12:16:02
tags: [flex, 前端, CSS]
categories: 前端
---
>不管是什么布局，Flex往往都可以几行命令搞定。

![preview](http://blogpic.at15cm.com/flex1-0.png)


## 骰子的布局
骰子的一面，最多可以放置9个点。

![flex-dice](http://blogpic.at15cm.com/flex1-1.png)

下面，就来看看Flex如何实现，从1个点到9个点的布局。你可以到 [codepen](http://codepen.io/LandonSchropp/pen/KpzzGo) 查看Demo。

![flex-dice](http://blogpic.at15cm.com/flex1-2.png)

如果不加说明，本节的HTML模板一律如下。

```
<div class="box">
	<span class="item"></span>
</div>
```

上面代码中，`div` 元素（代表骰子的一个面）是 `Flex` 容器，`span` 元素（代表一个点）是 `Flex` 项目。如果有多个项目，就要添加多个 `span` 元素，以此类推。

### 单项目
首先，只有左上角1个点的情况。Flex布局默认就是首行左对齐，所以一行代码就够了。

![flex-dice](http://blogpic.at15cm.com/flex1-3.png)

```
.box{
	display: flex;
}
```

设置项目的对齐方式，就能实现居中对齐和右对齐。

![flex-dice](http://blogpic.at15cm.com/flex1-4.png)

```
.box{
	display: flex;
	justify-content: center;
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-5.png)

```
.box{
	display: flex;
	justify-content: flex-end;
}
```

设置交叉轴对齐方式，可以垂直移动主轴。

![flex-dice](http://blogpic.at15cm.com/flex1-6.png)

```
.box{
	display: flex;
	align-items: center;
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-6.png)

```
.box{
	display: flex;
	justify-content: flex-end;
	align-items: center;
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-7.png)

```
.box{
	display: flex;
	justify-content: center;
	align-items: center;
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-8.png)

```
.box{
	display: flex;
	justify-content: center;
	align-items: flex-end;
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-9.png)

```
.box{
	display: flex;
	justify-content: flex-end;
	align-items: flex-end;
}
```

### 双项目

![flex-dice](http://blogpic.at15cm.com/flex1-10.png)

```
.box{
	display: flex;
	justify-content: space-between;
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-11.png)

```
.box{
	display: flex;
	flex-direction: column;
	justify-content: space-between;
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-12.png)

```
.box{
	display: flex;
	flex-direction: column;	
	justify-content: space-between;
	align-items: center;
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-13.png)

```
.box{
	display: flex;
	flex-direction: column;	
	justify-content: space-between;
	align-items: flex-end;
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-14.png)

```
.box{
	display: flex;
}
.item:nth-child(2) {
	align-self: center;
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-15.png)

```
.box{
	display: flex;
	justify-content: space-between;
}
.item:nth-child(2) {
	align-self: flex-end;
}
```

### 三项目

![flex-dice](http://blogpic.at15cm.com/flex1-16.png)

```
.box{
	display: flex;
}
.item:nth-child(2) {
	align-self: center;
}
.item:nth-child(3) {
	align-self: flex-end;
}
```

### 四项目

![flex-dice](http://blogpic.at15cm.com/flex1-17.png)

```
.box{
	display: flex;
	flex-wrap: wrap;
	justify-content: flex-end;
	align-content: space-between
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-18.png)

`HTML` 代码：

```
<div class="box">
  <div class="column">
    <span class="item"></span>
    <span class="item"></span>
  </div>
  <div class="column">
    <span class="item"></span>
    <span class="item"></span>
  </div>
</div>
```

`CSS` 代码：

```
.box{
	display: flex;
	flex-wrap: wrap;
	align-content: space-between
}
.column {
  flex-basis: 100%;
  display: flex;
  justify-content: space-between;
}
```

### 六项目

![flex-dice](http://blogpic.at15cm.com/flex1-19.png)

```
.box{
	display: flex;
	flex-wrap: wrap;
	align-content: space-between
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-20.png)

```
.box{
	display: flex;
	flex-direction;
	flex-wrap: wrap;
	align-content: space-between
}
```

![flex-dice](http://blogpic.at15cm.com/flex1-21.png)

`HTML` 代码：

```
<div class="box">
  <div class="row">
    <span class="item"></span>
    <span class="item"></span>
    <span class="item"></span>
  </div>
  <div class="row">
    <span class="item"></span>
  </div>
  <div class="row">
     <span class="item"></span>
     <span class="item"></span>
  </div>
</div>
```

`CSS` 代码：

```
.box {
  display: flex;
  flex-wrap: wrap;
}
.row{
  flex-basis: 100%;
  display:flex;
}
.row:nth-child(2){
  justify-content: center;
}
.row:nth-child(3){
  justify-content: space-between;
}
```

### 九项目


![flex-dice](http://blogpic.at15cm.com/flex1-22.png)

```
.box{
	display: flex;
	flex-wrap: wrap;
```


## 网格布局
### 基本网格布局
最简单的网格布局，就是平均分布。在容器里面平均分配空间，跟上面的骰子布局很像，但是需要设置项目的自动缩放。

![flex-grid](http://blogpic.at15cm.com/flex1-23.png)

`HTML` 代码：

```
<div class="Grid">
  <div class="Grid-cell">...</div>
  <div class="Grid-cell">...</div>
  <div class="Grid-cell">...</div>
</div>
```

`CSS` 代码：

```
.Grid {
  display: flex;
}
.Grid-cell {
  flex: 1;
}
```

### 百分比布局
某个网格的宽度为固定的百分比，其余网格平均分配剩余的空间。

![flex-grid](http://blogpic.at15cm.com/flex1-24.png)

`HTML` 代码：

```
<div class="Grid">
  <div class="Grid-cell u-1of4">...</div>
  <div class="Grid-cell">...</div>
  <div class="Grid-cell u-1of3">...</div>
</div>
```

`CSS` 代码：

```
.Grid {
  display: flex;
}
.Grid-cell {
  flex: 1;
}
.Grid-cell.u-full {
  flex: 0 0 100%;
}
.Grid-cell.u-1of2 {
  flex: 0 0 50%;
}
.Grid-cell.u-1of3 {
  flex: 0 0 33.3333%;
}
.Grid-cell.u-1of4 {
  flex: 0 0 25%;
}
```


## 圣杯布局
圣杯布局（Holy Grail Layout）指的是一种最常见的网站布局。页面从上到下，分成三个部分：头部（header），躯干（body），尾部（footer）。其中躯干又水平分成三栏，从左到右为：导航、主栏、副栏。

![holy grail](http://blogpic.at15cm.com/flex1-25.png)

`HTML` 代码：

```
<body class="HolyGrail">
  <header>...</header>
  <div class="HolyGrail-body">
    <main class="HolyGrail-content">...</main>
    <nav class="HolyGrail-nav">...</nav>
    <aside class="HolyGrail-ads">...</aside>
  </div>
  <footer>...</footer>
</body>
```

`CSS` 代码：

```
.HolyGrail {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}
header,
footer {
  flex: 1;
}
.HolyGrail-body {
  display: flex;
  flex: 1;
}
.HolyGrail-content {
  flex: 1;
}
.HolyGrail-nav, .HolyGrail-ads {
  /* 两个边栏的宽度设为12em */
  flex: 0 0 12em;
}
.HolyGrail-nav {
  /* 导航放到最左边 */
  order: -1;
}
```

>补充：相对于视口的高度。视口被均分为100单位的 `vh`

如果是小屏幕，躯干的三栏自动变为垂直叠加。

```
@media (max-width: 768px) {
	.HolyGrail-body {
		flex-direction: column;
		flex: 1;
	}
	.HolyGrail-nav,
	.HolyGrail-ads,
	.HolyGrail-content {
		flex: auto;
	}
}
```


## 输入框的布局
我们常常需要在输入框的前方添加提示，后方添加按钮。

![input](http://blogpic.at15cm.com/flex1-26.png)

`HTML` 代码

```
<div class="InputAddOn">
	<span class="InputAddOn-item">...</span>
	<input class="InputAddOn-field">
	<button class="InputAddOn-item">...</button>
</div>
```

`CSS` 代码

```
.InputAddOn {
	display: flex;
}
.InputAddOn-field {
	flex: 1;
}
```

## 悬挂式布局
有时，主栏的左侧或右侧，需要添加一个图片栏。

![media](http://blogpic.at15cm.com/flex1-27.png)

`HTML` 代码

```
<div class="Media">
  <img class="Media-figure" src="" alt="">
  <p class="Media-body">...</p>
</div>
```

`CSS` 代码

```
.Media {
  display: flex;
  align-items: flex-start;
}
.Media-figure {
  margin-right: 1em;
}
.Media-body {
  flex: 1;
}
```

## 固定的底栏
有时，页面内容太少，无法占满一屏的高度，底栏就会抬高到页面的中间。这时可以采用Flex布局，让底栏总是出现在页面的底部。

![footer](http://blogpic.at15cm.com/flex1-28.png)

`HTML` 代码

```
<body class="Site">
  <header>...</header>
  <main class="Site-content">...</main>
  <footer>...</footer>
</body>
```

`CSS` 代码

```
.Site {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}
.Site-content {
  flex: 1;
}
```


## 流式布局
每行的项目数固定，会自动分行。

![footer](http://blogpic.at15cm.com/flex1-29.png)

```
.parent {
  width: 200px;
  height: 150px;
  background-color: black;
  display: flex;
  flex-flow: row wrap;
  align-content: flex-start;
}
.child {
  box-sizing: border-box;
  background-color: white;
  flex: 0 0 25%;
  height: 50px;
  border: 1px solid red;
}
```

