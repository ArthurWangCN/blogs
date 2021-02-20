---
title: chart.js中的坑
date: 2017-07-22 22:31:37
tags: [插件, 前端]
categories: 工具
---
>**chart.js** 是一款网页中绘制图表的 JavaScript 框架，能够根据需要画出各种图表，而且文档全面细致，值得一用。

下面介绍一个自己使用过程中遇到的坑，给后来者提个醒。

**问题：**

```
chart.js(intermediate value).Line is not a function 
```

**原因：**
这是由于 `chart.js` 代码版本和其说明文档版本不匹配造成的。
下载的代码版本是 `v2.6.0`，使用的说明文档却是 `v1.xx` 的文档。

代码下载：<http://www.bootcss.com/p/chart.js/>
文档链接：<http://www.bootcss.com/p/chart.js/docs/>，这个文档没有具体说明是针对哪个代码版本的文档，容易让人误入歧途。

推荐使用官网地址：
<https://github.com/chartjs/Chart.js>
<http://www.chartjs.org/docs/>


**正确使用方法：**

```
new Chart(ctx, {
    type:'line',
    data: data
});
```

所以以后使用库，一定要注意使用的是哪个版本。



**参考：**
<https://github.com/reactjs/react-chartjs/issues/112>