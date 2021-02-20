---
title: AS3：函数
date: 2018-09-12 15:08:11
tags: AS3
categories: 游戏开发
---
## 函数的定义
### 函数语句定义法
使用 `function` 关键字
```
function add(a:int, b:int):int {
  return a + b;
}
```

this关键字牢牢指向当前函数定义的域

### 函数表达式定义法

```
var add:Function = function(a:int, b:int):int {
  return a + b;
}
```

this关键字随着附着的对象不同而改变


call和apply只能改变函数表达式定义法函数的this指向


## 访问参数信息

通过 arguments 对象：

+ arguments.length:参数长度
+ arguments.callee():持有指向当前函数的引用，用来创建递归


关键字...(rest)用来接收任意多参数，以数组形式保留在rest中


## 函数高级使用技巧
### 代理函数对象
设立一个代理函数对象，根据条件不同指向不同的函数，实现动态改变。

```
var mainFunc:Function;
var sex:String = "male";
if(sex == "male") {
  mainFunc = maleFunc;
} else {
  mainFunc = femaleFunc;
}

function maleFunc(){ //... }
function femaleFunc(){ //... }
```

### 建立函数执行队列

```
var funcAry:Array = new Array();

funcAry.push(aFunc);
funcAry.push(bFunc);
funcAry.push(cFunc);

for(var i:Number=0;i<funcAry.length;i++){
  funcAry[i]();
}

function aFunc() { //... }
function bFunc() { //... }
function cFunc() { //... }
```


### 利用函数返回函数

```
function chooseFuncBy(input:*){
  if(!(input is String)) {
    return objectFunc;
  }

  switch(input) {
    case "a":
      return aFunc;
    case "b":
      return bFunc;
    case "c":
      return cFunc;
    default: 
      return defaultFunc;
  }
}
```


### 函数动态添加属性
例如用函数动态属性计算函数调用次数

开火函数，记录执行次数
```
var shot:Function =  function ():void{
  shot['time']++;
  trace("shot():time:",shot['time']);
}
shot['time'] = 0;
shot();		//1
shot();		//2
shot();		//3
```


### 函数对象动态添加实例方法

开火超过3次，自动调用reload
```
var shot:Function =  function ():void{
  shot['time']++;
  trace("shot():time:",shot['time']);
  shot['reload']();
}
shot['time'] = 0;
shot['reload'] = function() {
  if(this['time'] > 3) {
    this['time'] = 0;
  }
}

```