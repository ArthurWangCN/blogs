---
title: 略谈const
date: 2017-04-26 17:44:53
tags: JavaScript
categories: 前端
---
>`const` 声明创建一个只读的常量。这不意味着常量指向的值不可变，而是变量标识符的值只能赋值一次。(译者注：JavaScript中的常量和Java，C++中的常量一个意思。注意区分常量的值和常量指向的值的不同)

## 语法
```
const name1 = value1 [, name2 = value2 [, ... [, nameN = valueN]]];
```

**nameN**
    常量名称，可以是任意合法的identifier（标识符）。
**valueN**
    常量值，可以是任意合法的表达式。

## 描述
这个声明创建了一个常量，可以在全局作用域或者函数内声明常量，常量需要被初始化。这就是说，在定义常量的同时必须初始化(这是有意义的，鉴于变量的值在初始化后就不能改变)。

常量拥有块作用域，和使用 `let` 定义的变量十分相似。常量的值不能通过再赋值改变，也不能再次声明。

一个常量不能和它所在作用域内的其他变量或函数拥有相同的名称。

示例：
下面的例子演示了常量的特性。在浏览器的控制台试一下这个例子。

```
// 注意: 常量在声明的时候可以使用大小写，但通常情况下会使用全部大写英文。 
// 定义常量MY_FAV并赋值7
const MY_FAV = 7;
// 在 Firefox 和 Chrome 这会失败但不会报错(在 Safari这个赋值会成功)
MY_FAV = 20;
// 输出 7
console.log("my favorite number is: " + MY_FAV);
// 尝试重新声明会报错 
const MY_FAV = 20;
//  MY_FAV 保留给上面的常量，这个操作会失败
var MY_FAV = 20; 
// MY_FAV 依旧为7
console.log("my favorite number is " + MY_FAV);
// 下面是一个语法错误
const A = 1; A = 2;
// 常量要求一个初始值
const FOO; // SyntaxError: missing = in const declaration
// 常量可以定义成对象
const MY_OBJECT = {"key": "value"};
// 重写对象和上面一样会失败
MY_OBJECT = {"OTHER_KEY": "value"};
// 对象属性并不在保护的范围内，下面这个声明会成功执行
MY_OBJECT.key = "otherValue";
```

## 兼容性说明
在Firefox和Chrome更早期的版本，Safari 5.1.7和Opera 12.00，如果使用const定义一个变量，这个变量的值仍然可以修改。IE6-10 不支持 const，但是IE11支持。


## js三种定义变量方式的区别
>js中三种定义变量的方式const， var， let的区别。

1.const定义的变量不可以修改，而且必须初始化。

```
const b = 2;//正确
// const b;//错误，必须初始化 
console.log('函数外const定义b：' + b);//有输出值
// b = 5;
// console.log('函数外修改const定义b：' + b);//无法输出 
```

2.var定义的变量可以修改，如果不初始化会输出undefined，不会报错。

```
var a = 1;
// var a;//不会报错
console.log('函数外var定义a：' + a);//可以输出a=1
function change(){
	a = 4;
	console.log('函数内var定义a：' + a);//可以输出a=4
} 
change();
console.log('函数调用后var定义a为函数内部修改值：' + a);//可以输出a=4
```

3.let是块级作用域，函数内部使用let定义后，对函数外部无影响。

```
let c = 3;
console.log('函数外let定义c：' + c);//输出c=3
function change(){
	let c = 6;
	console.log('函数内let定义c：' + c);//输出c=6
} 
change();
console.log('函数调用后let定义c不受函数内部定义影响：' + c);//输出c=3
```