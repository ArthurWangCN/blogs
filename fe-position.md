---
title: 关于 CSS 定位
date: 2018-09-01 22:35:45
tags: [前端, CSS]
categories: 前端
---

>CSS 定位 (Positioning) 属性允许你对元素进行定位。


## CSS 定位机制
CSS 有三种基本的定位机制：普通流、浮动和绝对定位。

除非专门指定，否则所有框都在普通流中定位。


## CSS position 属性
>通过使用 position 属性，我们可以选择 4 种不同类型的定位，这会影响元素框生成的方式。

### static
>元素框正常生成。块级元素生成一个矩形框，作为文档流的一部分，行内元素则会创建一个或多个行框，置于其父元素中。

### relative（相对定位）
>元素框偏移某个距离。元素仍保持其未定位前的形状，它原本所占的空间仍保留。

相对定位是一个非常容易掌握的概念。如果对一个元素进行相对定位，它将出现在它所在的位置上。然后，可以通过设置垂直或水平位置，让这个元素“相对于”它的起点进行移动。

```
#box_relative{
  position: relative; 
  left: 30px;
  top: 20px;
}
```

![relative](http://www.w3school.com.cn/i/ct_css_positioning_relative_example.gif)


### absolute
>元素框从文档流完全删除，并相对于其包含块定位。包含块可能是文档中的另一个元素或者是初始包含块。元素原先在正常文档流中所占的空间会关闭，就好像元素原来不存在一样。元素定位后生成一个块级框，而不论原来它在正常流中生成何种类型的框。

绝对定位使元素的位置与文档流无关，因此不占据空间。这一点与相对定位不同，相对定位实际上被看作普通流定位模型的一部分，因为元素的位置相对于它在普通流中的位置。

```
#box_absolute{
  position: absolute; 
  left: 30px;
  top: 20px;
}
```

![absolute](http://www.w3school.com.cn/i/ct_css_positioning_absolute_example.gif)

绝对定位的元素的位置相对于**最近的已定位祖先元素**，如果元素没有已定位的祖先元素，那么它的位置相对于最初的包含块。


### fixed
>元素框的表现类似于将 position 设置为 absolute，不过其包含块是视窗本身。
提示：相对定位实际上被看作普通流定位模型的一部分，因为元素的位置相对于它在普通流中的位置。



## CSS 浮动
浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。

由于浮动框不在文档的普通流中，所以文档的普通流中的块框表现得就像浮动框不存在一样。

在 CSS 中，我们通过 float 属性实现元素的浮动。

浮动相关见 [W3School](http://www.w3school.com.cn/css/css_positioning_floating.asp)