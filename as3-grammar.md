---
title: AS3：语言及语法
date: 2018-06-22 16:48:47
tags: AS3
categories: 游戏开发
---
## 对象和类
ActionScript 3.0 引入无类型变量这一概念，这一类变量可通过以下两种方法来指定：

``` 
var someObj:*; 
var someObj;``` 

无类型变量与 Object 类型的变量不同。二者的主要区别在于无类型变量可以保存特殊值 undefined，而 Object 类型的变量则不能保存该值。

使用 class 关键字来定义自己的类。

在方法声明中，可通过以下三种方法来声明类属性 (property)：
1. 用 const 关键字定义 常量，
2. 用 var 关键字定义变量，
3. 用 get 和 set 属性 (attribute) 定义 getter 和 setter 属性 (property)。

可以用 function 关键字来声明方法。

可使用 new 运算符来创建类的实例。下面的示例创建 Date 类的一个名为 myBirthday 的实例。

```
var myBirthday:Date = new Date();```


## 包和命名空间



