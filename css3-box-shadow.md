---
title: CSS3 box-shadow 属性详解
date: 2020-02-06 16:23:49
tags: CSS
categories: CSS
---

>CSS3定义了两种阴影：盒子阴影和文本阴影。其中盒子阴影需要IE9及其更新版本，而文本阴影需要IE10及其更新版本。下面分别介绍box-shadow阴影的使用：

## 盒子阴影box-shadow

box-shadow属性向box添加一个或多个阴影。

语法：

```
box-shadow: offset-x offset-y blur spread color inset;
box-shadow: X轴偏移量 Y轴偏移量 [阴影模糊半径] [阴影扩展] [阴影颜色] [投影方式];
```

词解释：blur:模糊 spread:伸展 inset:内凹

参数解释：

+ offset-x：必需，取值正负都可。offset-x水平阴影的位置。
+ offset-y：必需，取值正负都可。offset-y垂直阴影的位置。
+ blur:可选，只能取正值。blur-radius阴影模糊半径，0即无模糊效果，值越大阴影边缘越模糊。
+ spread：可选，取值正负都可。spread代表阴影的周长向四周扩展的尺寸，正值，阴影扩大，负值阴影缩小。
+color:可选。阴影的颜色。如果不设置，浏览器会取默认颜色，通常是黑色，但各浏览器默认颜色有差异，建议不要省略。可以是rgb(250,0,0)，也可以是有透明值的rgba(250,0,0,0.5)。
+ inset:可选。关键字，将外部投影(默认outset)改为内部投影。inset 阴影在背景之上，内容之下。

注意：inset 可以写在参数的第一个或最后一个，其它位置是无效的。


## box-shadow 使用

### 水平垂直偏移为0也可以有阴影

如果offset-x或offset-y值为0，则阴影在元素背后，此时给blur-radius值或spread值可以产生阴影效果。

但是有一点要注意：扩展阴影必须和阴影模糊半径配合使用。

我个人觉得应该是没有配合使用这一说，但不可能只设置扩展阴影，因为扩展阴影和阴影模糊的取值都可以为正。如果只有扩展阴影的话，会被浏览器当做模糊阴影来解析，所以也可以简单理解为“扩展阴影必须和阴影模糊半径配合使用”，如果只用扩展阴影，可以写成： `box-shadow:0 0 0 1px;`。

代码示例：

```
<style>
    div{
          width: 100px;
          height: 100px;
          margin:50px;
          border: 10px dotted red;
          display: inline-block;
  }
  .blur{
          box-shadow: 0 0  20px ;
          /*box-shadow: 0 0  20px green;*/ /*也可以自定义颜色*/
  }  
  .spread-positive{
          box-shadow: 0 0 20px 5px ;
          /* box-shadow: 0 0 20px 5px green;*/ /*也可以自定义颜色*/
  }
  .spread-negative{
          box-shadow: 0 0 20px -5px ;
          /* box-shadow: 0 0 20px -5px green;*/ /*也可以自定义颜色*/
  }
  </style>
</head>
<body>
  <div class="blur"></div>
  <div class="spread-positive"></div>
  <div class="spread-negative"></div>
</body>
```

### 设置水平垂直偏移得到阴影效果

outset情况：水平垂直偏移为0，但是不设置blur和spread，看不到阴影，因为此时box-shadow的周长和border-box一样，所以可以通过设置偏移让阴影显示出来。

inset情况：水平垂直偏移为0，不设置blur和spread，同样看不到阴影，因为此时box-shadow的周长和padding-box一样，同样可通过设置偏移让阴影显示出来。

代码示例：

```
<style type="text/css">
div{
    width: 100px;
    height: 100px;
    margin:50px;
    border: 10px dotted pink;
    display: inline-block;
}
.shadow0{box-shadow: 0 0;}  
.shadow1{box-shadow: 1px 1px;}
.shadow10{box-shadow: 10px 10px;}
.inset-shadow0{box-shadow: 0 0 inset;}  
.inset-shadow1{box-shadow: 1px 1px inset;}
.inset-shadow10{box-shadow: 10px 10px inset;}
</style>
<body>
    <div class="shadow0"></div>
    <div class="shadow1"></div>
    <div class="shadow10"></div>
    <div class="inset-shadow0"></div>
    <div class="inset-shadow1"></div>
    <div class="inset-shadow10"></div>
</body>
```


### 投影方式

投影方式默认是outset，即外部投影，可设置inset让向内投影。

例子：第一个div默认outset，第二个设置inset，第三个同时设置两个阴影可以更好的看到outset和inset的关系，第四个div可以看出inset阴影在背景之上，内容之下。

```
<style type="text/css">
div{
    width: 100px;
    height: 100px;
    margin:50px;
    border: 10px dotted pink;
    display: inline-block;
    vertical-align: top;
} 
.outset{
    box-shadow: 10px 10px teal;
}
.inset{
    box-shadow: 10px 10px teal inset;    
}
.double{
    box-shadow: 10px 10px teal inset,10px 10px teal;
}
.bg{
    background-color: yellow;
}
</style>
<body>
    <div class="outset"></div>
    <div class="inset"></div>
    <div class="double"></div>
    <div class="inset bg">inset阴影在背景之上，内容之下</div>
</body>
```


### 如果元素同时指定border-radius属性，则阴影呈现相同的圆角

```
<style type="text/css">
 div{
 width: 100px;
    height: 100px;
    margin:50px;
    border: 10px dotted pink;
    display: inline-block;
    border-radius: 50px;
 }
.shadow{
    box-shadow: 0 0  10px 10px green;
}
</style>
<body>
<div class="shadow"></div>
</body>
```

### 经典例子

[w3c中的一个例子](http://www.w3.org/TR/css3-background/#the-box-shadow)

+ border-radius会以相同的作用影响阴影外形
+ border-image,padding不会影响阴影的任何外形
+ 阴影box和box模型一样
+ 外阴影在对象背景之下，内阴影在背景之上。
+ 层次：内容>内阴影>背景图片>背景颜色>外阴影


### 多重阴影

语法：可以设置任意多个阴影，用逗号隔开。

一个box有多重阴影时，需要注意顺序：多个阴影从上往下分布，第一个阴影在最顶层。

举例：单边阴影效果

*先解释一下：可单独设置左边框的阴影，右边框的阴影，上边框的阴影，下边框的阴影，其实这样说也对，因为效果看起来就是这样，但根本原因是阴影在盒子后面，只是让阴影的位置发生了变化，其他3 个边的阴影还是存在的，只是被覆盖住了而已，所以，设置某个边的阴影是个很虚的东东了，哎，网上这种说法初看还让我略感困惑，所以我这里说是单边阴影效果，告诉大家只是一种效果，本质还是个box。*

例子解释：给第一个div的上右下左border分别设置红橙黄绿，四种颜色，则red-shadow在最顶层，green-shadow在最底层，如下图左。
给其加上blur模糊半径，效果更明显，如下图中，可见red-shadow的模糊半径不受干扰，因为在最顶层；接下来orange-shadow次之，被red-shadow的radius干扰；yellow-shadow被orange-shadow和red-shadow的radius干扰；同理green-shadow被它上面的所有shadow的radius干扰。

如果还是不太理解，那给red-shadow设置一个很大的radius，比如50，就可以看到非常明显的效果了。

```
<style type="text/css">
div{
    width: 100px;
    height: 100px;
    margin:50px;
    display: inline-block;
    border: 10px dotted pink;
}
.shadow{
    box-shadow: 0 -5px red,
    5px 0 orange,
    0 5px yellow,
    -5px 0 green;
}
.blur-shadow{
    box-shadow: 0 -5px 5px red,
    5px 0 5px orange,
    0 5px 5px yellow,
    -5px 0 5px green;
}
.big-redShadow{
    box-shadow: 0 -5px 50px red,
    5px 0 5px orange,
    0 5px 5px yellow,
    -5px 0 5px green;
}
</style>
<body>
    <div class="shadow"></div>
    <div class="blur-shadow"></div>
    <div class="big-redShadow"></div>
</body>
```

### 阴影和布局

阴影不影响布局， 但是可能会覆盖其他box或者其他box的阴影。

阴影不触发滚动条，也不增加滚动区域的大小。

所以布局时可忽略阴影。

### spread妙用

用spread模拟实现border

```
<style type="text/css">
div{
    width: 100px;
    height: 100px;
    display: inline-block;
    margin:10px;
    vertical-align: top;
}
.border{
    border:1px solid red;
}
.spread{
    box-shadow: 0 0 0 1px red;
}
.muli-border{
    box-shadow: 0 0 0 2px red,0 0 0 4px green,0 0 0 6px blue;
}
</style>
<body>
    <div class="border">border</div>
    <div class="spread">box-shadow</div>
    <div class="muli-border">多重<br/>box-shadow</div>
</body>
```

用spread实现双色方括号

```
<style type="text/css">
.decorator {
width: 300px;
height: 100px;
padding: 30px;
box-shadow: -30px -30px 0 -25px red,30px 30px  0 -25px green; 
}
</style>
<body>
<div class="decorator">段落内容：用box-shadow模拟双色方括号box-shadow: -24px -24px 0 -20px red,24px 24px  0 -20px green; </div>
</body>
```



本文来自 [「巧克力很苦」](https://blog.csdn.net/qq_44607694/article/details/89605857)

一些链接：

+ [The box-shadow Property](https://bitsofco.de/the-box-shadow-property/)