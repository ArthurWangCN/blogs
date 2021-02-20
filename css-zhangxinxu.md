---
title: CSS深入理解（张鑫旭）
date: 2019-06-11 00:13:40
tags: [前端, CSS]
categories: 前端
---
## border
### 利用border实现三大杠布局

![三大杠](http://blogpic.at15cm.com/3hyphen.png)

巧妙利用 `border-style: double` 属性可以实现三大杠布局。

计算规则：

![计算规则](http://blogpic.at15cm.com/borderstylerules.png)

代码：

```
width: 120px;
height: 20px;
border-top: 60px double;
border-bottom: 20px solid;
```


### border-color 与 color
border-color 默认颜色就是 color（未指定border-color就是使用color作为颜色）。

类似的还有box-shadow,text-shadow。

应用：
常用的添加图片的按钮，悬浮上之后变颜色。

![添加图片](http://blogpic.at15cm.com/addPicBtn.gif)

```
<a href="javascript:;" class="add"></a>
```

```
.add {
	color: #ccc;
	transition: color .25s;
	border: 1px solid;
}
.add::before {
	border-top: 10px solid;
}
.add::after {
	border-left: 10px solid;
}
.add:hover {
	color: #06c;
}
```

 
### border与三角形构建

当border-width值很大的时候可以看到每一边是一个梯形：

```
.rect {
	width: 100px; height: 100px;
	border: 100px solid;
	border-color: red greenyellow aquamarine burlywood;
}
```

![border与梯形](http://blogpic.at15cm.com/border-rect.png)

当上例的 width 和 height 均为0时就变成了三角形：

![border与三角形](http://blogpic.at15cm.com/border-square.png)

通过 `border-color: red transparent transparent transparent` 就可以得到一个倒立的三角形：

![border与三角形](http://blogpic.at15cm.com/border-triangle.png)



## float
float的设计初衷仅仅是为了实现 **文字环绕效果**。

浮动具有破坏性，会让父元素高度塌陷。

**清除浮动（带来的影响）的方法**：

方法一：脚底插入 `clear: both`
	1.HTML block水平元素底部
	
	2.CSS after伪元素

方法二：父元素BFC(IE8+)或haslayout(IE6/IE7)

权衡后的策略：

```
.clearfix {
	content: '';
	display: block;
	height: 0;
	overflow: hidden;
	clear:both;
}
.clearfix { *zoom: 1; }
```

更好的方法：

```
.clearfix {
	content: '';
	display: table;
	clear:both;
}
.clearfix { *zoom: 1; }
```


## absolute

relative 和 absolute 是分离的、对立的，并且absolute越独立越强大。

独立的absolute可以摆脱overflow的限制，无论是滚动还是隐藏。

