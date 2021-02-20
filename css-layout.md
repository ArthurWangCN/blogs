---
title: 前端一些布局问题
date: 2019-06-25 00:06:01
tags: [前端, CSS]
categories: 前端
---
## 等高布局

![等高布局](http://blogpic.at15cm.com/css-layout1.png)

### flex实现

```
<div class="box1">
	<nav class="left1">
		<h3>nav1</h3>
		<h3>nav2</h3>
	</nav>
	<section class="right1">
		<p>module1</p>
		<p>module2</p>
		<p>module3</p>
		<p>module4</p>
        </section>
</div>
```

```
.box1{
	display: flex;
	width: 800px;
	background-color: #222;
}
.left1{
	flex: 1;
	background-color: #222;
}
.right1{
	flex: 1;
	background-color: #ccc;
}
```

**优点：** 简便、完整、响应式
**缺点：** IE9及IE9以下版本不支持flex属性


### table-cell实现

```
.box{
	width: 800px;
}
.left, 
.right{
	display: table-cell;
	width: 400px;
	baground-color: #ccc;
}
```

**优点：** 兼容IE8+
**缺点：** 对margin无感，设置margin无效


### 使用内外边距相抵消实现

注意父元素设置 `overflow: hidden;`

```
<div id="container">
	<div id="left" class="column aside">
		<p>Sidebar</p>
	</div>
	<div id="content" class="column section">
		<p>Main content；content；content；content；content</p>
	</div>
	<div id="right" class="column aside">
		<p>Sidebar</p>
	</div>
</div>
```

```
#container {
	margin: 0 auto;
	overflow: hidden;
	width: 960px;
}
.column {
    background: #ccc;
    float: left;
    width: 200px;
    margin-right: 5px;
    margin-bottom: -99999px;
    padding-bottom: 99999px;
}
#content {
    background: #eee;
}
#right {
    float: right;
    margin-right: 0;
}
```

**优点：** 兼容所有浏览器
**缺点：** 页面高度超出设置的padding-bottom后会有问题