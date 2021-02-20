---
title: LayaAir：鼠标事件详解
date: 2018-06-04 10:12:18
tags: [LayaAir, 游戏开发]
categories: 游戏开发
---

**LayaAir的鼠标事件有以下特点：**

+ 默认精灵对象的是不接受鼠标事件的（mouseEnabled=false）。
+ 给精灵设置任意鼠标事件监听，会自动打开自己和所有父对象的鼠标事件（mouseEnabled=true）。
+ 某个对象想被点击中，需要符合两个条件：mouseEnabled=true，有宽高或者hitArea属性，默认精灵是不设置宽高的，但Laya自带的UI系统会自动计算宽高，所以一般无需设置宽高。
+ 鼠标事件会冒泡，也就是子对象被命中，父对象也会收到相应的事件，无论父对象宽高是多少（可以通过event.stopPropagation()阻止事件冒泡）。
+ 鼠标事件检测流程：先检测子对象是否命中，然后才检测父对象是否命中。
+ 如果想忽略空白穿透点击，可以设置对象的mouseThrough=true，这样只会点击有东西的地方，空白的地方会穿透下去。
+ 事件基于对象池实现，最大程度复用对象，全局也只有一个Event实例，所以使用时不要引用event对象本身，可以引用event内部属性，比如：
`var evt = event;`（不建议这样写）
`var target = event.target`（建议这样写）



**事件使用代码示例：(以js写法为例，其他语言写法类同)**

+ 增加一个事件监听

	`sp.on(Event.CLICK, this, onSpClick);`

    或者click代替Event.CLICK常量，下面类同

	`sp.on(“click”, this, onSpClick);`

+ 移除一个事件

	`sp.off(Event.CLICK, this, onSpClick);`

	移除一个对象所有类型为click的事件

	`sp.offAll(Event.CLICK);`

+ 移除一个对象身上的所有事件

	`sp.offAll();`

+ 监听某个事件一次，监听被触发后会被移除

	`sp.once(Event.CLICK, this, onSpClick);`


+ 事件监听携带自定义数据，并返回

	```
	sp.on(Event.CLICK, this, onSpClick,["parm1",2,"parm3"]);
	//自定义数据以参数的形式，放到前面，默认事件返回在后面
	function onSpClick(parm1, parm2, parm3,e) {
	  console.log(parm1);
	  console.log(parm2);
	  console.log(parm3);
	}```


+ 是否监听过某种事件

	`sp.hasListener(Event.CLICK);`

+ 派发一个事件

	`sp.event(Event.CLICK, "参数");`

+ 自定义事件示例

	```
	//监听一个自定义事件
	sp.on(“myevent”,this,onMyEvent)
	function onMyEvent(parm){
	  console.log(parm);
	}
	//派发一个自定义事件
	sp.event(“myevent”,”this is a parm”);```


+ 自定义多参数自定事件
	```
	sp.on(“myevent”,this,onMyEvent)
	function onMyEvent(parm1,parm2,parm3){
	  console.log(parm1);
	  console.log(parm2);
	  console.log(parm3);
	  //派发一个多参数自定义事件
	  sp.event(“myevent”,[”this is a parm”,2,3]);
	}```


通过上面的示例，可以看出LayaAir的事件吸收了flash，node.js和u3d的一些特点，接口以简洁为目标：比如on的写法，非常简短，易用：比如offAll，once，自定义事件等，高性能：比如对象池复用等，学习成本低，性能高。

