---
title: setTimeout 与 setInterval
date: 2018-08-10 23:31:56
tags: [前端, JavaScript]
categories: 前端
---
想要隔一段时间触发一段代码，定时器是最方便的实现，前端中的定时器方法是浏览器提供的，并不是 ECMAScript 规范中的，是 window 对象的方法，定时器有 `setInterval` 和 `setTimeout`。

## 定义
setInterval 和 setTimeout
那么又到了万年不变的问题，setInterval和setTimeout有什么区别？

废话，当然有区别了，名字不一样。

骚不骚

咳咳，错了错了，其实想要区别很简单，从本质上看吧：

一种定时器是 每间隔一定时间 执行一次，循环往复。比如每隔一秒执行一次，六十秒过后执行了60次。`
另一种定时器是 过了一定时间 执行一次，只执行一次。比如隔一秒后执行一次，过了十万八千秒后也只在第一秒执行了一次，仅有的一次。
第一种是：setInterval；第二种是：setTimeout。

这两个方法的参数是一模一样的：

正常使用的话，需要有两个参数。
都可以传递无数个参数。
第一个参数是要执行的js语句，第二参数是用来描述时间，以毫秒为单位。
↓↓↓如果你是大佬，下面不用看。actually，整篇文章都不用看，右上角关闭了解下。

```
// 第一个参数的第一种情况
var timer1 = setTimeout("console.log('I am Pelli')");
// 第一个参数的第二种情况
var timer2 = setTimeout(function(){
    console.log("I am Pelli");
});
// 第一个参数的第三种情况
function showMyName(){
    console.log("I am Pelli");
}
var timer3 = setTimeout(showMyName);
// ！注意：由于以上三种情况都没有传递控制时间的第二个参数，第二个参数默认是0，都会在0秒后执行。以上三种方式的效果一模一样
// 第一个参数的第四种情况【实际开发中基本上无用】
var timer4_0 = setTimeout(null);
var timer4_1 = setTimeout(12);
var timer4_2 = setTimeout({});//Uncaught SyntaxError: Unexpected identifier
var timer4_3 = setTimeout([]);
var timer4_4 = setTimeout(NaN);
var timer4_5 = setTimeout(false);
var timer4_6 = setTimeout(/I am Pelli/);
```

## 清除定时器
`clearInterval()` 和 `clearTimeout()` 方法用来清楚定时器，往函数里传定时器的名字即可

定时器先触发一次再延时
项目中经常有这样的需求，比如我这次接实时在线接口，需要加载后执行一次，然后每五分钟执行一次，就用到了这个。

用一个小demo来演示一下：

```
var data2=0;
var count2= function(){
  console.log("count2:",data2++);
  return count2;//若不返回时，此函数只会执行一次
}
setInterval(count2(),1000);
```

也可写成：

```
var data3=0;
(function count3(){
   console.log("count3:",data3++);
   setTimeout(count3,1000);
})();
```

当然你要说先让函数执行一次再写定时部分我也赞同。

问题不大