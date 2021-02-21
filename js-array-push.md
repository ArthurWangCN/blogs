---
title: 数组的push()、pop()、shift()和unshift()方法
date: 2020-02-02 16:14:36
tags: JavaScript
categories: [前端, JavaScript]
---
>JavaScript的数组是一个拥有堆栈和队列自身优点的global对象。也就是说JavaScript数组可以表现的像栈(LIFO)和队列(FIFO)一样操作。这也是JavaScript数组强大的可操作性的体现。

## 堆栈和队列
栈和队列都是动态的集合，在栈中，可以去掉的元素是最近插入的那一个。栈实现了后进先出。在队列中，可以去掉的元素总是在集合中存在的时间最长的那一个。队列实现了先进先出的策略。

### 堆栈的基本概念

先上张图：

![堆栈](http://q513dozlr.bkt.clouddn.com/js-stack.png)

ECMAScript为数组专门提供了 `shift()` 和 `unshift()` 方法，以便实现类似队列的行为。由于 `push()` 是向数组末端添加数组项的方法，因此要模拟队列只需一个从数组前端取得数组项的方法。实现这一操作的数组方法就是 `shift()` ，它能够移除数组中的第一个项并返回该项，同时将数组长度减1。

顾名思义， `unshift()` 与 `shift()` 的用途相反：它能在数组前端添加任意个数组项并返回新数组的长度。因此，同时使用 `unshift()` 和 `pop()` 方法，可以从相反的方向来模拟队列，即在数组的前端添加数组项，从数组末端移除数组项。

简单得回忆一下：

+ `push()` 方法可以在数组的末属添加一个或多个元素
+ `shift()` 方法把数组中的第一个元素删除
+ `unshift()`方法可以在数组的前端添加一个或多个元素
+ `pop()` 方法把数组中的最后一个元素删除


### 实现类似栈的行为
将 `push()` 和 `pop()` 结合在一起，我们就可以实现类似栈的行为：

```
//创建一个数组来模拟堆栈
var a = new Array();
console.log(a);
//push: 在数组的末尾添加一个或更多元素，并返回新的长度
console.log("入栈");
a.push(1);
console.log(a);     //----->1
a.push(2);
console.log(a);     //----->1,2
a.push(3);
console.log(a);     //----->1,2,3
a.push(4);
console.log(a);     //----->1,2,3,4
console.log("出栈 后进先出");
console.log(a);
//pop: 从数组中把最后一个元素删除，并返回这个元素的值
a.pop();
console.log(a);     //----->4
a.pop();
console.log(a);     //----->3
a.pop();
console.log(a);     //----->2
a.pop();
console.log(a);     //----->1
```

在Chrome浏览器控制台输出的效果如下图所示：

```
[]
入栈
[1]
[1, 2]
[1, 2, 3]
[1, 2, 3, 4]
出栈 后进先出
[1, 2, 3, 4]
[1, 2, 3]
[1, 2]
[1]
[]
```

### 实现类似队列的行为
将 `shift()` 和 `push()` 方法结合在一起，可以像使用队列一样使用数组。即在数组的后端添加项，从数组的前端移除项：

```
//创建一个数组来模拟队列
var a = new Array();
console.log(a);
//push: 在数组的末尾添加一个或更多元素，并返回新的长度
console.log("入栈");
a.push(1);
console.log(a);     //----->1
a.push(2);
console.log(a);     //----->1,2
a.push(3);
console.log(a);     //----->1,2,3
a.push(4);
console.log(a);     //----->1,2,3,4
console.log("出队 先进先出");
console.log(a);
//shift: 从数组中把第一个元素删除，并返回这个元素的值
a.shift();
console.log(a);     //----->1
a.shift();
console.log(a);     //----->2
a.shift();
console.log(a);     //----->3
a.shift();
console.log(a);     //----->4
```

在Chrome浏览器控制台输出的效果如下图所示：

```
[]
入队
[1]
[1, 2]
[1, 2, 3]
[1, 2, 3, 4]
出队 先进先出
[1, 2, 3, 4]
[2, 3, 4]
[3, 4]
[4]
[]
```

除此之外，还可以同时使用 `unshift()` 和 `pop()` 方法，从相反的方向来模拟队列，即在数组的前端添加项，从数组的后端移除项。

本文来自 [luck_lin](https://blog.csdn.net/qwe502763576/article/details/79055682)


