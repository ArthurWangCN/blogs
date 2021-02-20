---
title: AS3：数据类型
date: 2018-09-06 17:05:38
tags: AS3
categories: 游戏开发
---
>ActionScript3 数据类型分为基本数据类型和复杂数据类型。

## 数据类型
### 基本数据类型
+ Boolean
+ int —— 处理整数
+ Number —— 处理很长又有小数点的数字
+ String
+ uint —— 很大的正整数


### 复杂数据类型
+ Array
+ Date
+ Error
+ Function
+ RegExp
+ XML
+ XMLList
+ 自己定义的类


## 变量的声明
使用 `var` 关键字

```
var 变量名:数据类型;
var 变量名:数据类型 = 值;
```

注：不加上数据类型的声明被归为未声明类型（untyped）。

*了解值类型和引用类型*

## 常量的声明 
使用 `const` 关键字

```
const foo:int = 100;
```

对于值类型来说，常量持有的就是值；对于引用类型来说，常量持有的是引用。

对引用类型，常量只能保证引用不会变，但不能保证引用对象本身的状态不会发生变化。


## 基础（非基本）数据类型
+ 所有的基元数据类型：Boolean、int、uint、Number、String
+ 两种复杂数据类型：Array、Object

Boolean如果未赋值，默认为 `false`。


## 变量的默认值

+ int： 0
+ uint: 0
+ Number: NaN
+ String: null
+ Boolean: false
+ Array: null
+ Object: null
+ 未声明类型： undefined


## 运算符

等于运算符执行数据类型转换，而全等不会：

```
var a:int = 5;
var b:uint = 5;
trace(a == b);		//true
trace(a === b);		//true，都是数值类型

var c:String = "5";
trace(a == c);		//true，执行了数据转换
trace(a === c);		//false

var d:int = 1;
var e:Boolean = true;
trace(d == e);		//true，e被转成了数值1
trace(d === e);		//false
```


两边运算对象都是 Number 类型，且值为NaN，全等和等于都判断为false：

```
var a:Number;
var b:Number;
trace(a);		//NaN
trace(b);		//NaN
trace(a == b);		//false
trace(a === b);		//false
```

undefined 和 null 等于运算符返回true，全等返回false：

```
trace(undefined == null);		//true
trace(undefined === null);		//false
```

关系运算符如果一边是数值一边是非数值会先转成数值再判断：

```
var a:int = 5;
var b:String = "4";
trace(a > b);		//true，执行了数据转换
```

如果非数值无法转换成数值，这个表达式永远为false：

```
var a:int = 5;
var b = "number";
trace(a > b);		//fal
```


## typeof,is,as

+ typeof是用字符串形式返回对象的类型：

```
trace(typeof 10);		//number
```

+ is用来判断一个对象是否属于一种类型：

```
trace(9 is Number);		//true
```

+ as:如果一个对象属于一种类型，那么as返回这个对象；否则返回null

```
trace(9 is Number);		//9
trace(9 is Array);		//null
```