---
title: Javascript Basic
date: 2019-11-18 21:43:06
tags: JavaScript
categories: 前端
---
## 原型和原型链
### 构造函数
```
function Foo(name, age) { 
	this.name = name; 
	this.age = age; 
	this.class = 'class-1'; 
	//return this 
}
var f = new Foo('Karlie', 20);
```


1. `var a = {}` 其实是 `var a = new Object()` 的语法糖；

2. `var a = []` 其实是 `var a = new Array()` 的语法糖；

3. `function Foo(){...}` 其实是 `var Foo = new Function(...)` 的语法糖；

4. 使用 `instanceof` 判断一个函数是否是一个变量的构造函数，例如：`var arr = []; arr instanceof Array;`


### 原型规则
1. 所有的引用类型（数组、对象、函数），都具有对象特性，即可自由扩展属性（null 除外）；

2. 所有的引用类型（数组、对象、函数）， 都有一个 __proto__ 属性，属性值是一个普通的对象；

3. 所有的函数，都有一个prototype属性，属性值也是一个普通的对象；

4. 所有的引用类型（数组、对象、函数）， __proto__ 属性值指向它的构造函数 prototype 属性值。

```
//可自由扩展属性
var obj = {}; obj.a = 100;
var arr = []; arr.a = 100;
function fn () {}
fn.a = 100;
//所有的引用类型都有一个 __proto__ 属性
console.log(obj.__proto__);
console.log(arr.__proto__);
console.log(fn.__proto__);
//所有的函数都有一个prototype属性
console.log(fn.prototype);
//__proto__ 属性值指向它的构造函数 prototype 属性值
console.log(obj.__proto__ === Object.prototype);
```
5. 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么它会去它的 `__proto__`（即它的构造函数的prototype）中寻找。

```
//构造函数
function Foo(name, age) { this.name = name; }
Foo.prototype.alertName = function() { alert(this.name); }
//实例
var f = new Foo('kk');
f.printName = function() { console.log(this.name); }
f.printName();
f.alertName(); 
```

循环对象自身的属性：

```
var item;
for(item in f) {
	if(f.hasOwnProperty(item)) {
		console.log(item);
	}
}
```

### 原型链

![原型链](http://blogpic.at15cm.com/prototype.png)

### instanceof
`f instaceof Foo` 的判断逻辑：
1. f 的 `__proto__` 一层一层往上，能否对应到 `Foo.prototype`
2. 再试着判断 `f instanceof Object`

### 一个简单原型链继承的例子
```
function Animal() {
	this.eat = function() { console.log('animal eat') }
}
function Dog() {
	this.bark = function() { console.log('dog bark') }
}
Dog.prototype = new Animal();
var d = new Dog();
```

### new一个对象的过程
1. 创建一个对象
2. this指向这个新对象
3. 执行代码，即对this赋值
4. 返回this


## 作用域和闭包
### 执行上下文
范围：一段 `<script>` 或者一个函数；
全局：变量定义、函数声明；
函数：变量定义、函数声明、this、arguments。

注意函数声明和函数表达式的区别。

```
console.log(a);		//undefined
var a = 200;
fn('kk');		//kk 20
function fn(name) {
	var age = 20;
	console.log(name, age);
} 
```
### this
this要在执行时才能确认值，定义时无法确认。

```
var a = {
	name: "A",
	fn: function() {
		console.log(this.name);
	}
}
a.fn();		//this === a
a.fn.call({name: 'B'});		//this === {name: 'B'}
var fn1 = a.fn;
fn1();		//this === window
```

1.作为构造函数执行

```
function Foo(name) {
	//this = {}
	this.name = name;
	//return this;
}
var f = new Foo('Mozz');
```

2.作为对象属性执行

```
var obj = {
	name: 'Mozz',
	printName: function() {
		console.log(this.name);		//this === obj
	}
}
obj.printName();
```

3.作为普通函数执行

```
function fn() {
	console.log(this);		//this === window
}
```

4.call apply bind

```
function fn1(name) {
	alert(name);
	console.log(this);
}
fn1.call({x:100}, 'Mozz');	//this === {x:100}
fn1.apply({x:100}, ['Mozz']);	//和call的区别是后面是数组
var fn2 = function(name, age) {
	alert(name);
	console.log(this);
}.bind({y:200})
fn2("Mozz", 27);
```

### 作用域
+ JS没有块级作用域；
+ 只有函数和全局作用域

```
//无块级作用域
if(true){
	var name = "Mozz";
}
console.log(name);		//Mozz
//函数和全局作用域
var a = 100;
function fn() {
	var a = 200;
	console.log('fn', a);
}
console.log('global', a);	//global 100
fn();	//fn 200
```

### 作用域链

```
var a = 100;
function fn() {
	var b = 200;
	//当前作用域没有定义的变量，即自由变量
	console.log(a);
	console.log(b);
}
fn();
```
当前作用域没有，就向父级作用域找。
在什么地方定义，父级作用域就是谁。

### 闭包
使用场景：
1.函数作为返回值
```
function F1() {
	var a = 100;
	return function() {
		console.log(a);
	}
}
var f1 = F1();
var a = 200;
f1();	//100
```

2.函数作为参数
```
function F1() {
	var a = 100;
	return function() {
		console.log(a);
	}
}
var f1 = F1();
function F2(fn) {
	var a = 200;
	fn();
}
F2(f1);		//100
```

### 一些问题

1、变量提升
变量定义
函数声明（注意和函数表达式的区别）

2、this的使用场景

3、如何理解作用域
+ 自由变量
+ 作用域链，即自由变量的查找
+ 闭包的两个场景

4、闭包的应用

```
//实际主要用于封装变量，收敛权限
function isFirstLoad() {
	var _list = [];
	return function(id) {
		if(_list.indexOf(id) >= 0) {
			return false;
		} else {
			_list.push(id);
			return true;
		}
	}
}
var firstLoad = isFirstLoad();
firstLoad(1);	//true
firstLoad(1);	//false
firstLoad(2);	//true
firstLoad(2);	//false
```

## 异步和单线程
###异步
```
console.log(100);
setTimeout(function() {
	console.log(200);
},1000)
console.log(300);	//100 300 200
```

和同步的区别是不阻塞程序的进行。

前端使用异步的场景：
1. 定时任务：setTimeout、setInterval；
2. 网络请求：Ajax请求、动态img加载；
3. 事件绑定。

```
//ajax请求
console.log('start');
$.get('./data.json',function(){
	console.log(data);
})
console.log('end');
//img动态加载
console.log('start');
var img = document.createElement('img');
img.onload = function(){
	console.log('loaded');
}
img.src = './img.png'
console.log('end');		//start end loaded
//事件绑定
console.log('start');
btn.addEventListener('click',function(){
	console.log('clicked');
})
console.log('end');		//start end clicked
```

### 单线程
```
console.log(100);
setTimeout(function() {
	console.log(200);
},0)
console.log(300);	//100 300 200
```

+ 执行第一行，打印100
+ 执行setTimeout后，传入setTimeout的函数会被暂存起来，不会立即执行（单线程特点，不能同时干两件事）
+ 执行最后一行，打印300
+ 待所有程序执行完，处于空闲状态时，立刻看有没有暂存起来要执行的
+ 发现暂存起来的setTimeout中的函数等待时间为0立即执行







