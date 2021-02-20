---
title: ES6：Promise 对象
date: 2020-05-05 18:08:39
tags: [ES6, promise]
categories: ES6
---

## Promise的含义

> Promise 是异步编程的一种解决方案 。 所谓 `Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。  从语法上说，  Promise 是一个对象，从它可以获取异步操作的消息。 

`Promise`对象有以下两个特点：

（1）对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。



`Promise`也有一些缺点。首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。 



## 基本用法

 ES6 规定，`Promise`对象是一个构造函数，用来生成`Promise`实例。 

```javascript
const promise = new Promise(function(resolve, reject) {
    // some code ...
    if (/* 异步操作成功 */) {
    	resolve(value)    
    } else {
        reject(error)
    }
})
```

+  `resolve` ：将`Promise`对象的状态从 pending 变为 resolved， 在异步操作成功时调用，并将异步操作的结果，作为参数传递出去； 
+  `reject`：将`Promise`对象的状态从 pending 变为 rejected，在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。 



 `Promise`实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数。 



+  立即 resolved 的 Promise 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务 ；
+  p2的resolve方法将p1作为参数 ， 这时p1的状态就会传递给p2，即p1的状态决定了p2的状态；
+  调用`resolve`或`reject`并不会终结 Promise 的参数函数的执行，所以最好在前面加return。 



## Promise.prototype.then()

> Promise 实例具有`then`方法，也就是说，`then`方法是定义在原型对象`Promise.prototype`上的。它的作用是为 Promise 实例添加状态改变时的回调函数。 

+ `then`方法返回的是一个新的`Promise`实例（注意，不是原来那个`Promise`实例）；
+ 第一个参数是`resolved`状态的回调函数；
+ 第二个参数（可选）是`rejected`状态的回调函数 。



## Promise.prototype.catch()

> `Promise.prototype.catch()`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。 



+  `reject()`方法的作用，等同于抛出错误 ；
+  如果 Promise 状态已经变成`resolved`，再抛出错误是无效的 ；
+  Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。即错误总是会被下一个`catch`语句捕获 ；
+  建议总是使用`catch()`方法，而不使用`then()`方法的第二个参数 ；
+  `catch()`方法返回的还是一个 Promise 对象 ；
+  如果没有报错，则会跳过`catch()`方法；
+  如果没有使用`catch()`方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应；
+  Node.js 有一个`unhandledRejection`事件，专门监听未捕获的`reject`错误 。





## Promise.prototype.finally()

> `finally()`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。 

```javascript
promise
.then(result => { ... })
.catch(err => { ... })
.finally(() => { ... })
```



+ 不管`promise`最后的状态，在执行完`then`或`catch`指定的回调函数以后，都会执行`finally`方法指定的回调函数；
+  `finally`方法无参数，所以里面的操作应该是与状态无关的，不依赖于 Promise 的执行结果；
+  `finally`本质上是`then`方法的特例；
+  `finally`方法总是会返回原来的值；



## Promise.all()

> `Promise.all()`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。 

```javascript
const p = Promise.all([p1, p2, p3])
```

+ 接受一个数组作为参数，p1、p2、p3 都是 Promise 实例；
+ 如果不是，先调用 `Promise.resolve` 转为 Promise实例；
+ p1、p2、p3 都变成 fulfilled，p的状态才为 fulfilled，返回值组成一个数组，传递给p的回调函数；
+ p1、p2、p3 有一个被 rejected，p的状态就变成 rejected，第一个被rejected的实例的返回值，传给p的回调；
+  如果作为参数的 Promise 实例，自己定义了`catch`方法，那么它一旦被`rejected`，并不会触发`Promise.all()`的`catch`方法。



## Promise.race() 

> `Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。 

只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数。 



## Promise.allSettled()

> Promise.allSettled() ：只有等到所有这些参数实例都返回结果，不管是`fulfilled`还是`rejected`，包装实例才会结束。该方法由 [ES2020](https://github.com/tc39/proposal-promise-allSettled) 引入 



## Promise.any()

> Promise.any()： 只要参数实例有一个变成`fulfilled`状态，包装实例就会变成`fulfilled`状态；如果所有参数实例都变成`rejected`状态，包装实例就会变成`rejected`状态。 



## Promise.resolve()

> 将现有对象转为 Promise 对象 

```javascript
Promise.resolve('foo');
// 等价于
new Promise(resolve => resolve('foo'))
```

 `Promise.resolve`方法的参数分成四种情况：

**（1）参数是一个 Promise 实例**

如果参数是 Promise 实例，那么`Promise.resolve`将不做任何修改、原封不动地返回这个实例。

**（2）参数是一个`thenable`对象**

+ `thenable`对象指的是具有`then`方法的对象
+  `Promise.resolve`方法会将这个对象转为 Promise 对象，然后就立即执行`thenable`对象的`then`方法 ；
+  `thenable`对象的`then`方法执行后，对象`p1`的状态就变为`resolved`，从而立即执行最后那个`then`方法指定的回调函数 。

**（3）参数不是具有`then`方法的对象，或根本就不是对象**

如果参数是一个原始值，或者是一个不具有`then`方法的对象，则`Promise.resolve`方法返回一个新的 Promise 对象，状态为`resolved`。

**（4）不带有任何参数**

`Promise.resolve()`方法允许调用时不带参数，直接返回一个`resolved`状态的 Promise 对象。



注意：立即`resolve()`的 Promise 对象，是在本轮“事件循环”（event loop）的结束时执行，而不是在下一轮“事件循环”的开始时 。



## Promise.reject()

> `Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected` 

 注意，`Promise.reject()`方法的参数，会原封不动地作为`reject`的理由，变成后续方法的参数。这一点与`Promise.resolve`方法不一致。 



## Promise.try()

> 让同步函数同步执行，异步函数异步执行 

 **第一种**写法是用`async`函数来写：

```javascript
const f = () => console.log('now');
(async () => f())();
console.log('next');
// now
// next
```

 **第二种**写法是使用`new Promise()` ：

```javascript
const f = () => console.log('now');
(
  () => new Promise(
    resolve => resolve(f())
  )
)();
console.log('next');
// now
// next
```



有一个提案：Promise.try

```javascript
const f = () => console.log('now');
Promise.try(f);
console.log('next');
// now
// next
```





本文来自 [ECMAScript 6 入门 —— 阮一峰](https://es6.ruanyifeng.com/#docs/promise)