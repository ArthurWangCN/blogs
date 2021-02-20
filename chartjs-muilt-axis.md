---
title: chart.js多坐标系的实现
date: 2018-06-05 10:55:33
tags: [插件, 工具, 前端]
categories: 工具
---
> 想要统计一些骑行数据，第一时间想到了Chart.js，之前用过还是挺方便。但是骑行时间和骑行距离数值相差太大，就在想这么强大的图表绘制工具库肯定有多坐标系的功能。没有找到API，倒是找到了 [实例](https://chartjs.bootcss.com/samples/charts/line/multi-axis.html)，查看源码自己整理吧。


### 第一步：引入Chart.js

常规操作，想用人家的东西，肯定得引人家的库


### 第二步：搭结构

代码如下：

```
<div class="chart-container" style="position: relative; height:40vh; width:80vw">
  <canvas id="yourId"></canvas>
</div>```

说明： `style` 属性是为了响应式，可以不写， `yourId` 替换成你想用的id，但是记得在下一步JS代码里也要改成相同的id。


### 第三步：写逻辑

代码如下：

```
var BarChartData = {
  labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
  datasets: [{
    label: 'My First dataset',
    borderColor: "#FF9966",
    backgroundColor: "#FF9966",
    fill: false,
    data: [0, 28.43, 27.29, 31.50, 24.52, 35.53, 13.53],
    yAxisID: 'y-axis-1',
  }, {
    label: 'My Second dataset',
    borderColor: "#99CCCC",                
    backgroundColor: "#99CCCC",
    fill: false,
    data: [0, 6.1, 5.6, 5.8, 5.3, 6.3, 4.5],
    yAxisID: 'y-axis-2'
  }]
};
window.onload = function() {
  var ctx = document.getElementById('yourId').getContext('2d');
  window.myLine = Chart.Bar(ctx, {
    data: BarChartData,
    options: {
      responsive: true,
      hoverMode: 'index',
      stacked: false,
      title: {
        display: true,
        text: 'Chart.js Bar Chart - Multi Axis'
      },
      scales: {
        yAxes: [{
          type: 'linear',
          display: true,
          position: 'left',
          id: 'y-axis-1',
        }, {
          type: 'linear',
          display: true,
          position: 'right',
          id: 'y-axis-2',
          // grid line settings
          gridLines: {
            drawOnChartArea: false,
          },
        }],
      }
    }
  });
};```


### 关键点

**yAxisID** , **scales**

主要是加上y坐标的ID，在初始化的时候把y坐标轴也初始化。



### 扩展

+ [Chart.js -- 图表绘制工具库](http://legendaryarthur.cn/2017/07/22/chart-js/)
+ [Chart.js 实例](https://chartjs.bootcss.com/samples/)

