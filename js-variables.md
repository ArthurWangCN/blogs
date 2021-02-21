---
title: 几个js变量题
date: 2020-04-13 10:44:05
tags: JavaScript
categories: [前端, JavaScript]
---

**1、程序的运行结果为：100  10  100**

```javascript
var a = 10;
function test() {
	a = 100;
	console.log(a);
	console.log(this.a);
	var a;
	console.log(a);
}
test();
```

解析：Javascript在执行前会对整个脚本文件的声明部分做完整分析(包括局部变量)，从而确定变量的作用域，所以在函数test执行前，由于第6行声明了局部变量a，所以函数内部的a都指向已经声明的局部变量，所以第4行输出100。第5行输出this.a，我们都知道，函数内部的this指针指向的是函数的调用者，在这里函数test被全局对象调用，所以this指针指向全局对象（这里即window），所以this.a = window.a，一开始生命了全局变量a=10，所以第5行输出结果为10。第7行输出结果为100，因为局部变量a在第3行已经被赋值了100，所以直接输出局部变量a的值。



**2、程序的运行结果为：undefined  10**

```javascript
var a = 100;
function test() {
	console.log(a);
	var a = 10;
	console.log(a);
}
test();
```

解析：看了第1个例子，可能有同学会认为输出结果是10  10，但是结果却不是10 10，为什么呢？仔细看第1个例子解析的第一句话，Javascript在执行前会对整个脚本文件的**声明部分**做完整分析(包括局部变量)，**但是不能对变量定义做提前解析**，在这个函数中，执行第3行前，可以认为已经声明了变量a，但是并没有定义（这里即赋值），所以第3行输出结果为undefined，执行第4行a =10后，变量a的值就为10，所以第5行输出结果为10。



**3、程序的运行结果为：100  10  10**

```javascript
var a = 100;
function test() {
  console.log(a);
  a = 10;
  console.log(a);
}
test();
console.log(a);
```

解析：我们知道在函数内部，一般用var声明的为局部变量，没用var声明的一般为全局变量，在test函数内，a=10声明了一个全局变量，所以第3行的a应该输出全局变量的值，而在函数执行之前已经声明过一个全局变量并赋值100，所以这里第上输出100。第4行给全局变量a 重新赋值10，所以全局变量a的值变成10，所以第5行输出10。而在函数test外部，第8行输出全局变量a的值，因为全局变量被重新赋值为10，所以输出结果即为10。



**4、程序的运行结果为：10**

```javascript
var foo=1;
function bar(){
	if(!foo){
		var foo=10;
	}
	console.log(foo);
}
bar();
```

解析：if和for里的var定义的变量也是会提升的；if 里面的 var 和 function的声明方式，都会存在变量提升，但是并不赋值。





**参考：**

+ [JS变量提升题集](https://blog.csdn.net/weixin_45562774/article/details/100567973)
+ [函数声明提升与变量声明提升](https://www.jianshu.com/p/44a3c76023f5)
+ [变量声明和变量提升](https://blog.csdn.net/Cathence/article/details/80516673)
+ [前端基础进阶（三）：变量对象详解](https://www.jianshu.com/p/330b1505e41d)

