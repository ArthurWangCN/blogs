---
title: axios
date: 2020-05-07 23:50:53
tags: [Vue, axios]
categories: [前端, Vue]
---





### 拦截器

#### 用法

在请求或响应被 `then` 或 `catch` 处理前拦截它们。

```javascript
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

如果你想在稍后移除拦截器，可以这样：

```javascript
var myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

可以为自定义 axios 实例添加拦截器:：

```javascript
var instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```

#### 实际场景

一些网站过了一定的时间不进行操作，就会退出登录让你重新登陆页面。

1、创建一个axios实例

```javascript
let instance = axios.create({
  headers: {
    'content-type': 'application/x-www-form-urlencoded'
  }
})
```

2、请求拦截器

这个拦截器会在你发送请求之前运行，功能是为我每一次请求去判断是否有token，如果token存在则在请求头加上这个token。后台会判断我这个token是否过期。

```javascript
// http request 拦截器
instance.interceptors.request.use(
  config => {
    const token = sessionStorage.getItem('token')
    if (token) { // 判断是否存在token，如果存在的话，则每个http header都加上token
      config.headers.authorization = token  //请求头加上token
    }
    return config
  },
  err => {
    return Promise.reject(err)
  }
 )
```

3、响应拦截器

```javascript
// http response 拦截器
instance.interceptors.response.use(
  response => {
    //拦截响应，做统一处理 
    if (response.data.code) {
      switch (response.data.code) {
        case 1002:
          store.state.isLogin = false
          router.replace({
            path: 'login',
            query: {
              redirect: router.currentRoute.fullPath
            }
          })
      }
    }
    return response
  },
  //接口错误状态处理，也就是说无响应时的处理
  error => {
    return Promise.reject(error.response.status) // 返回接口返回的错误信息
  }
)
```

4、导出实例

```javascript
export default instance
```

5、使用

```javascript
import instance from './axios'
function handleLogin(data) {
  return instance.post('/xx/user/login', data)
}
```



### 取消

使用 `cancel token` 取消请求

> Axios 的 cancel token API 基于[cancelable promises proposal](https://github.com/tc39/proposal-cancelable-promises)，它还处于第一阶段。

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

源码解析见 [使用CancelToken来取消请求](https://www.cnblogs.com/hahazexia/p/10001782.html)



### 官方API

+ [axios官方API](https://www.kancloud.cn/yunye/axios/234845)

+ [英文版](http://www.axios-js.com/docs/)

