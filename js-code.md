---
title: JS代码块
date: 2019-07-05 13:02:41
tags: JavaScript
categories: 前端
---


## 获取display值

代码：
```
var display = ele.currentStyle ? ele.currentStyle.display : window.getComputedStyle(ele, null).display;
```

### style对象

style对象代表一个单独的样式声明，可以从应用样式的文档元素访问Style对象。style对象获取的是内联样式，即元素标签中style属性的值。
`document.getElementById('id').style.color;`

### currentStyle对象

返回所有样式声明（包括内部、外部、内联）按css层叠规则作用于元素的最终样式。只有IE和Opera支持使用CurrentStyle获取的元素计算后的样式。`getComputedStyle()`方法可以获取当前元素所使用的css属性值。

```
var div=window.getComputedStyle("div",null).color;//第一个参数为目标元素，第二个参数为伪类（必需，没有伪类设为null）
```


与style对象的区别：

`getComputedStyle()`是只读，只能获取不能设置，style能读能设；

对于一个没有设定任何样式的元素，`getComputedStyle()`返回对象中的length属性值，而style对象中length是0。

currentStyle:该属性只兼容IE,不兼容火狐和谷歌;
getComputedStyle:该属性是兼容火狐谷歌,不兼容IE。


