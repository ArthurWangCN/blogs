---
title: 使用CancelToken来取消axios请求
date: 2020-05-08 00:45:32
tags: [Vue, axios]
categories: [前端, Vue]
---

### 官方API

#### CancelToken.source

可以使用 `CancelToken.source` 工厂方法创建 cancel token：

```javascript
var CancelToken = axios.CancelToken;
var source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function(thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
    // 处理错误
  }
});

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.');
```

#### 传一个executor函数

还可以通过传递一个 executor 函数到 `CancelToken` 的构造函数来创建 cancel token：

```javascript
var CancelToken = axios.CancelToken;
var cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// 取消请求
cancel();
```

可以使用同一个 cancel token 取消多个请求。



### 源码解析

**CancelToken.js**

```javascript
'use strict';

var Cancel = require('./Cancel');

/**
 * A `CancelToken` is an object that can be used to request cancellation of an operation.
 *
 * @class
 * @param {Function} executor The executor function.
 */
//CancelToken构造函数，CancelToken是一个用于取消操作的对象
//CancelToken跳页面后取消掉还在pending状态的请求以提高性能
function CancelToken(executor) {
  if (typeof executor !== 'function') {//如果executor不是function，抛出错误
    throw new TypeError('executor must be a function.');
  }

  var resolvePromise;
  this.promise = new Promise(function promiseExecutor(resolve) {
    resolvePromise = resolve;
  });
  //CancelToken对象上的promise属性，一个promise对象，resolve方法存下来到resolvePromise里

  var token = this;//当前CancelToken对象
  executor(function cancel(message) {
    if (token.reason) {
      //如果CancelToken对象上已经存在reason属性，说明已经请求过取消操作了
      // Cancellation has already been requested
      return;
    }

    token.reason = new Cancel(message);//给当前CancelToken对象上设置reason属性，是一个Cancel对象
    resolvePromise(token.reason);//调用resolve方法，参数是当前CancelToken对象上的reason属性
    //用户调用cancel方法后会对当前cancelToken对象上的promise改变其状态为resolve，会运行适配器中的resolve回调来终止请求
  });
}

/**
 * Throws a `Cancel` if cancellation has been requested.
 */
//如果取消操作已经请求过了，就抛出Cancel对象的信息，也就是CancelToken对象的reason属性
CancelToken.prototype.throwIfRequested = function throwIfRequested() {
  if (this.reason) {//如果当前CancelToken对象上有reason属性，直接抛出
    throw this.reason;
  }
};

/**
 * Returns an object that contains a new `CancelToken` and a function that, when called,
 * cancels the `CancelToken`.
 */
//CancelToken类上的source静态方法
//直接返回一个新建好的CancelToken对象和取消方法，方便使用，差不多就是工厂模式的意思
//返回一个对象包含两个属性，一个token属性是一个CancelToken新实例，另一个cancel属性是一个取消方法
//cancel取消方法对应CancelToken构造函数中的cancel方法，用户取消请求的时候直接调用就可以了
CancelToken.source = function source() {
  var cancel;
  var token = new CancelToken(function executor(c) {
    cancel = c;
  });
  return {
    token: token,
    cancel: cancel
  };
};

module.exports = CancelToken;
```



**Cancel.js**

```javascript
'use strict';

/**
 * A `Cancel` is an object that is thrown when an operation is canceled.
 *
 * @class
 * @param {string=} message The message.
 */
//Cancel对象构造函数，cancel对象是当请求被取消的时候会抛出的对象
function Cancel(message) {
  this.message = message;//Cancel对象上的message属性
}

Cancel.prototype.toString = function toString() {//返回将Cancel对象的message属性的字符串提示
  return 'Cancel' + (this.message ? ': ' + this.message : '');
};

Cancel.prototype.__CANCEL__ = true;//Cancel原型上的__CANCEL__内部属性，默认为true

module.exports = Cancel;
```

**isCancel.js**

```javascript
'use strict';

module.exports = function isCancel(value) {
  //判断请求失败时的错误对象是不是Cancel对象
  return !!(value && value.__CANCEL__);
};
```



### 项目中的应用

在vue中使用的时候可以新建一个全局变量数组，专门存放所有请求的cancel方法，在axios的请求拦截器中将cancel插入全局变量数组中，当路由切换的时候，在路由钩子里遍历这个数组，调用所有的cancel方法，将上一个页面还未完的请求取消，这样就可以加快页面的加载，避免不必要的等待。

```javascript
var CancelToken = axios.CancelToken;

axios.interceptors.request.use((config) => {
  //在请求拦截器中为每一个请求添加cancelToken，并将cancel方法存入全局数组中保存
  config.cancelToken = new CancelToken((c) => {
    Vue.prototype.__cancels__.push(c);
  });
  return config;
}, (error) => {
  return Promise.reject(error);
});

router.beforeEach((to, from, next) => {
  //当路由切换页面的时候，遍历全局数组，将上一个页面的所有请求cancel掉
  Vue.prototype.__cancels__.forEach((cancel) => {
    cancel();
  });
  Vue.prototype.__cancels__ = [];
})
```

