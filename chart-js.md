---
title: Chart.js -- 图表绘制工具库
date: 2017-07-22 21:42:30
tags: [插件, 前端]
categories: 工具
---
>**Chart.js**
是一个简单、面向对象、为设计者和开发者准备的图表绘制工具库。

![chart](http://blogpic.at15cm.com/chart-0.jpg)

## 起步
### 引入Chart.js文件
首先我们需要在页面中引入 `Chart.js` 文件。此工具库在全局命名空间中定义了 `Chart` 变量。

### 创建图表
为了创建图表，我们要实例化一个 `Chart对象`。为了完成前面的步骤，首先需要需要传入一个绘制图表的 `2d context`。以下是案例。

```
<canvas id="myChart" width="400" height="400"></canvas>
```

```
//Get the context of the canvas element we want to select
var ctx = document.getElementById("myChart").getContext("2d");
var myNewChart = new Chart(ctx).PolarArea(data);
```

我们还可以用 jQuery 获取 `canvas` 的 `context`。首先从 jQuery 集合中获取我们需要的 DOM 节点，然后在这个 DOM 节点上调用 `getContext("2d")` 方法。

```
//Get context with jQuery - using jQuery's .get() method.
var ctx = $("#myChart").get(0).getContext("2d");
//This will get the first returned node in the jQuery collection.
var myNewChart = new Chart(ctx);
```

当我们完成了在指定的 canvas 上实例化 Chart 对象之后，Chart.js 会自动针对 retina 屏幕做缩放。

Chart 对象设置完成后，我们就可以继续创建 Chart.js 中提供的具体类型的图表了。下面这个案例中，我们将展示如何绘制一幅极地区域图（Polar area chart）。


```
new Chart(ctx).PolarArea(data,options);
```


## 曲线图（Line chart）
### 简介
曲线图就是将数据标于曲线上的一种图表。

一般用于展示趋势数据，和比较两组数据集。

### 使用案例

![line chart](http://blogpic.at15cm.com/chart-1.png)

```
new Chart(ctx).Line(data,options);
```

### 数据结构

```
var data = {
	labels : ["January","February","March","April","May","June","July"],
	datasets : [
		{
			fillColor : "rgba(220,220,220,0.5)",
			strokeColor : "rgba(220,220,220,1)",
			pointColor : "rgba(220,220,220,1)",
			pointStrokeColor : "#fff",
			data : [65,59,90,81,56,55,40]
		},
		{
			fillColor : "rgba(151,187,205,0.5)",
			strokeColor : "rgba(151,187,205,1)",
			pointColor : "rgba(151,187,205,1)",
			pointStrokeColor : "#fff",
			data : [28,48,40,19,96,27,100]
		}
	]
}
```

### 图表参数

```
Line.defaults = {
				
	//Boolean - If we show the scale above the chart data			
	scaleOverlay : false,
	
	//Boolean - If we want to override with a hard coded scale
	scaleOverride : false,
	
	//** Required if scaleOverride is true **
	//Number - The number of steps in a hard coded scale
	scaleSteps : null,
	//Number - The value jump in the hard coded scale
	scaleStepWidth : null,
	//Number - The scale starting value
	scaleStartValue : null,

	//String - Colour of the scale line	
	scaleLineColor : "rgba(0,0,0,.1)",
	
	//Number - Pixel width of the scale line	
	scaleLineWidth : 1,

	//Boolean - Whether to show labels on the scale	
	scaleShowLabels : false,
	
	//Interpolated JS string - can access value
	scaleLabel : "<%=value%>",
	
	//String - Scale label font declaration for the scale label
	scaleFontFamily : "'Arial'",
	
	//Number - Scale label font size in pixels	
	scaleFontSize : 12,
	
	//String - Scale label font weight style	
	scaleFontStyle : "normal",
	
	//String - Scale label font colour	
	scaleFontColor : "#666",	
	
	///Boolean - Whether grid lines are shown across the chart
	scaleShowGridLines : true,
	
	//String - Colour of the grid lines
	scaleGridLineColor : "rgba(0,0,0,.05)",
	
	//Number - Width of the grid lines
	scaleGridLineWidth : 1,	
	
	//Boolean - Whether the line is curved between points
	bezierCurve : true,
	
	//Boolean - Whether to show a dot for each point
	pointDot : true,
	
	//Number - Radius of each point dot in pixels
	pointDotRadius : 3,
	
	//Number - Pixel width of point dot stroke
	pointDotStrokeWidth : 1,
	
	//Boolean - Whether to show a stroke for datasets
	datasetStroke : true,
	
	//Number - Pixel width of dataset stroke
	datasetStrokeWidth : 2,
	
	//Boolean - Whether to fill the dataset with a colour
	datasetFill : true,
	
	//Boolean - Whether to animate the chart
	animation : true,

	//Number - Number of animation steps
	animationSteps : 60,
	
	//String - Animation easing effect
	animationEasing : "easeOutQuart",

	//Function - Fires when the animation is complete
	onAnimationComplete : null
	
}
```


其他如柱状图、雷达图、基地区域图等都大致相同，这里不再赘述，详细见：[Chart.js 中文文档](http://www.bootcss.com/p/chart.js/docs/)