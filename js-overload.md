---
title: JavaScript中的函数重载
date: 2020-05-07 22:21:50
tags: JavaScript
categories: [前端, JavaScript]
---

### 什么是函数重载

重载函数是函数的一种特殊情况，为方便使用，C++允许在同一范围中声明几个功能类似的同名函数，但是这些同名函数的[形式参数](https://baike.baidu.com/item/形式参数/1995068)（指参数的个数、类型或者顺序）必须不同，也就是说用同一个函数完成不同的功能。这就是重载函数。重载函数常用来实现功能类似而所处理的数据类型不同的问题。不能只有函数返回值类型不同。

与之相似的函数重写

函数重写，也被称为覆盖，是指子类重新定义父类中有相同名称和参数的虚函数，主要在继承关系中出现。

### 函数重载基本条件

- 函数名必须相同；
- 函数参数必须不相同，可以是参数类型或者参数个数不同；
- 函数返回值可以相同，也可以不相同。（如果函数的名称和参数完全相同，仅仅是返回值类型不同，是无法进行函数重载的。）

### 函数重载应用场景

同一场景下，对于函数功能相同，仅仅参数不同的情况下进行重载，可减少开发的重复命名等情况。

### JavaScript中的函数重载

JavaScript 中没有真正意义上的函数重载，因为 JavaScript 中同一作用域下的同名函数，前者会被后者覆盖，但是可通过其他方法间接实现重载同样的效果，JavaScript中的函数没有签名，它的参数是由包含零的多个数组来表示的。无函数签名的话重载是不可能做到的。

但是我们可以实现重载效果，使用 [arguments 对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments)，是函数内部的一个类数组对象，它里面保存着调用函数时传递给函数的所有参数。 简单的讲就是使用逻辑判断，根据参数所在数组的长度来执行不同的代码。

重载的本质就是将多个功能相近的函数合并为同一个函数。

### 一个函数重载的实现

我们现在有这样的一个需求，有一个people对象，里面存着一些人名，如下：

```javascript
const people = {
	values: ["Dean Edwards", "Alex Russell", "Dean Tom"]
};
```

我们希望people对象拥有一个find方法，当不传任何参数时，就会把people.values里面的所有元素返回来；当传一个参数时,就把first-name跟这个参数匹配的元素返回来；当传两个参数时，则把first-name和last-name都匹配的才返回来。因为find方法是根据参数的个数不同而执行不同的操作的，所以，我们希望有一个addMethod方法，能够如下的为people添加find的重载：

```javascript
addMethod(people, "find", function() {}); /*不传参*/
addMethod(people, "find", function(a) {}); /*传一个*/
addMethod(people, "find", function(a, b) {}); /*传两个*/
```

这时候问题来了，这个全局的addMethod方法该怎么实现呢？John Resig的实现方法如下，代码不长，但是非常的巧妙：

```javascript
function addMethod(object, name, fn) {
　　var old = object[name]; //把前一次添加的方法存在一个临时变量old里面
　　object[name] = function() { // 重写了object[name]的方法
　　　　// 如果调用object[name]方法时，传入的参数个数跟预期的一致，则直接调用
　　　　if(fn.length === arguments.length) {
　　　　　　return fn.apply(this, arguments);
　　　　// 否则，判断old是否是函数，如果是，就调用old
　　　　} else if(typeof old === "function") {
　　　　　　return old.apply(this, arguments);
　　　　}
　　}
}
```

现在，我们一起来分析一个这个addMethod函数，它接收3个参数，第一个为要绑定方法的对象，第二个为绑定的方法名称，第三个为需要绑定的方法（一个匿名函数）。函数体的的分析已经在注释里面了。 OK，现在这个addMethod方法已经实现了，我们接下来就实现people.find的重载啦！全部代码如下：

```javascript
function addMethod(object, name, fn) {
    let old = object[name];
    object[name] = function () {
        if (fn.length === arguments.length) {
            return fn.apply(this, arguments);
        } else if (typeof old === "function") {
            return old.apply(this, arguments);
        }
    }
}
const people = {
    values: ["Dean Edwards", "Alex Russell", "Dean Tom"]
};
addMethod(people, 'find', function() {
    return this.values;
})
addMethod(people, 'find', function(anyName) {
    let ret = [];
    for (let index = 0; index < this.values.length; index++) {
        if (this.values[index].indexOf(anyName) >= 0) {
            ret.push(this.values[index]);
        }
    }
    return ret;
})
addMethod(people, 'find', function(firstName, lastName) {
    let ret = [];
    for (let index = 0; index < this.values.length; index++) {
        if (this.values[index] === `${firstName} ${lastName}`) {
            ret.push(this.values[index]);
        }
    }
    return ret;
})
console.log(people.find()); // ["Dean Edwards", "Alex Russell", "Dean Tom"]
console.log(people.find('Dean'));   // ["Dean Edwards", "Dean Tom"]
console.log(people.find('Dean', 'Tom'));    // ["Dean Tom"]
```



本文整理自 [JS - 实现重载](https://blog.csdn.net/qq_44352182/article/details/88876625)

