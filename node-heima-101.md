---
title: node 101
date: 2020-04-28 18:09:53
tags: Node
categories: Node
---



## package.json文件

### node_modules 文件夹的问题

1.  文件夹以及文件过多过碎，当我们将项目整体拷贝给别人的时候，传输速度很慢
2. 复杂的模块依赖关系需要被记录，确保模块的版本和当前保持一致，否则会导致当前项目运行报错

### package.json文件的作用

项目描述文件，记录了当前项目信息，例如项目名称、版本、作者、github地址、当前项目依赖了哪些第三方模块等。

使用 `npm init -y` 命令生成

### 项目依赖

+ 在项目的开发阶段和线上运营阶段，都需要依赖的第三方包，称为项目依赖
+ 使用 `npm install` 包名命令下载的文件会默认被添加到 `package.json` 文件的 `dependencies` 字段中

```json
{
	"dependencies": {
		"jquery": "^3.3.1"
	}
}
```

### 开发依赖

+ 在项目的开发阶段需要依赖，线上运营阶段不需要依赖的第三方包，称为开发依赖
+ 使用 `npm install 包名 --save-dev` 命令将包添加到 package.json 文件的 `devDependencies` 字段中

```json
{
	"devDependencies": {
        "gulp": "^3.9.1"
    }
}
```

### package-lock.json文件的作用

+ 锁定包的版本，确保再次下载时不会因为包版本不同而产生问题
+ 加快下载速度，因为该文件中已经记录了项目所依赖第三方包的树状结构和包的下载地址，重新安装时只需下载即可，不需要做额外工作

```json
"ansi-gray": {
    "version": "0.1.1",
    "resolved": "http://registry.npm.taobao.org/ansi-gray/download/....",
    "integrity": "sha1-xxxxxxx",
    "dev": "true",
    "require": {
        "ansi-wrap": "0.1.0"
    }
}
```



## Node.js模块加载机制

### 模块查找规则

#### 当前模块拥有路径但没有后缀时

```javascript
require('./find.js')	// 1
require('./find')		// 2
```

1. require 方法根据模块查找路径查找模块，如果是完整路径，直接引入模块
2. 如果模块后缀省略，先找同名 JS 文件，再找同名 JS 文件夹
3. 如果找到了同名文件夹，找文件夹中的 `index.js`
4. 如果文件夹中没有 index.js 就会去当前文件夹中的 `package.json` 文件中查找 `main` 选项中的入口文件
5. 如果找指定的入口文件不存在或者没有指定入口文件就会报错，模块没有找到

#### 当模块没有路径且没有后缀时

```javascript
require('find')
```

1. Node.js 会假设它是系统模块
2. Node.js 会去 `node_modules` 文件夹中
3. 首先看是否有该名字的 JS 文件
4. 再看是否有该名字的文件夹
5. 如果是文件夹看里面是否有 `index.js`
6. 如果没有 index.js 查看该文件夹中的 `package.json` 中的 `main` 选项确定模块入口文件
7. 否则找不到报错



---------



## 服务器端基础概念

### 网站的组成

网站的应用程序分为两大部分：客户端和服务器端

+ 客户端：浏览器中运行的部分，就是永辉看到并与之交互的界面程序，HTML、CSS、JS
+ 服务器端：在服务器中运行的部分，负责存储数据和处理应用逻辑

### Node网站服务器

能够提供网站访问服务的机器就是网站服务器，它能够接收客户端的请求，能够对请求做出响应。

+ 首先是一台电脑
+ 安装node环境
+ 使用node.js创建一个能够接收请求和响应请求的对象

### IP地址

互联网中设备的唯一标识

### 域名

由于IP地址难记，所以产生了域名的概念，也就是上网所使用的网址

虽然在地址栏中输的是网址，但是最后还是会转换成IP地址才能访问到指定的网站服务器。

### 端口

端口是计算机与外界通讯交流的出口，用来区分服务器电脑中提供的不同服务

### URL

统一资源定位符，是专为表示internet网上资源位置而设的一种编址方式

组成：`传输协议://服务器IP或域名:端口号/资源所在位置标识`



## 创建web服务器

```javascript
const http = require('http')
const app = http.createServer()
app.on('request', (req, res) => {
	res.end('<h1>hi</h1>')
})
app.listen(3000)
console.log('server running at http://localhost:3000')
```



## HTTP协议

### 概念

超文本传输协议，规定了如何从网站服务器传输超文本到本地服务器，基于客户端服务器架构工作，是客户端和服务器端请求和应答的标准。

### 报文

在HTTP请求和相应的过程中传递的数据块就叫报文，包括要传送的数据和一些附加信息，并且要遵守规定好的格式。

### 请求报文

1. 请求方式 `req.method`
   1. GET	请求数据
   2. POST   发送数据
2. 请求地址：`req.url`
3. 请求报文：`req.headers`

### 响应报文

1.HTTP状态码

+ 200：请求成功
+ 404：请求的资源没有被找到
+ 500：服务器端错误
+ 400：客户端请求有语法错误

```javascript
res.writeHead(200)
```



2.内容类型

+ text/html
+ text/css
+ application/javascript
+ image/jpeg
+ application/json

```javascript
res.writeHead(200, {
	'content-type': 'text/plain'	// 纯文本
})
```



## HTTP请求与响应处理

### 请求参数

客户端向服务器端发送请求时，有时候需要携带一些客户信息，通过请求参数的形式传递到服务器端。

### GET请求参数

+ 参数被放置在浏览器地址栏中：`localhost:3000/index?name=jack&age=23`

内置模块 `url` 用于处理url地址：

```javascript
// 1.要解析的url地址
// 2.true:将查询参数解析成对象形式
url.parse(req.url, true)
// search, query, pathname
```



### POST请求参数

+ 参数被放置在请求体中传输
+ 获取POST参数需要使用 `data` 事件和 `end` 事件
+ 使用 `querystring` 系统模块将参数转换为对象格式



```javascript
const querystring  = require('querystring')
app.on('request', (req, res) => {
    let postData = ''
    // 监听参数传输事件
    req.on('data', chunk => {
        postData += chunk
    })
    // 监听参数传输完毕事件
    req.on('end', () => {
        querystring.parse(postData)
    })
})
```



### 路由

**路由是指客户端请求地址与服务器程序代码的对应关系**。简单的说，就是请求什么响应什么。

```javascript
app.on('request', (req, res) => {
    // 请求方法
    const method = req.method.toLowerCase()
    // 请求地址
    const pathname = url.parse(req.url).pathname
    if (method === 'get') {
    	if (pathname === '/' || pathname === '/index') {
        	res.end('index page')
    	} else if (pathname === '/list') {
        	res.end('list page')
        } else {
            res.end('404')
        }
    } else if (method === 'post') {
        // ...
    }
})
```



### 静态资源

服务器不需要处理，可以直接响应给客户端的资源就是静态资源，例如CSS、JS、image文件。

```javascript
const http = require('http')
const url = require('url')
const path = require('path')
const fs = require('fs')
const mime = require('mime)

const app = http.createServer()
app.on('request', (req, res) => {
    let pathname = url.parse(req.url).path
    pathname = pathname === '/' ? '/default.html' : pathname
   	// 将用户请求路径转换为实际的服务器硬盘地址
    let realPath = path.join(__dirname, 'public', pathname)
    let type = mime.getType(realPath)
    // 读取文件
    fs.readFile(realPath, (error, result) => {
        if (error != null) {
            res.writeHead(404, {
                'content-type': 'text/html;charset=utf8'
            })
            res.end('404 page not found')
            return
        }
        res.writeHead(200, {
            'content-type': type
        })
        res.end(result)
    })
})
```



### 动态资源

相同的请求地址不同的响应资源，这种资源就是动态资源。



## Node.js异步编程

### 同步API 异步API

+ 同步：只有当前API执行完后，才能执行下一个API —— path.join、url.parse
+ 异步：当前API执行不会阻塞后续代码的执行 —— fs.readFile

### 区别

+ 同步API可以从返回值中拿到API执行的结果，但是异步不可以。

### 回调函数

自己定义函数让别人调用

```javascript
function getData (callback) { callback(xxx) }
getData((xxx) => {})
```

### Promise

promise的出现是为了解决Node.js异步编程中回调地狱的问题

```javascript
let promise = new Promise((resolve, reject) => {
    setTimeout(() => {
    	if (true) {
        	resolve({name: 'jack'})
    	} else {
        	reject('failed')
    	}
    }, 2000)
})
promise.then(result => console.log(result))		// {name: 'jack'}
	   .catch(error => console.log(error))		// failed
```

### 异步函数

> 异步函数是异步编程语法的终极解决方案，它可以让我们将异步代码写成同步的形式，让代码不再有回调函数嵌套，使代码变得清晰明了。

```javascript
const fn = async () => {}
async function fu () {}
```



**async关键字**

+ 在普通函数定义的前面加上 `async` 关键字，普通函数就变成了异步函数；
+ 异步函数默认返回值是Promise对象；
+ 异步函数内使用 `return` 关键字进行结果返回，结果会被包裹的promise对象中，return 关键字替代了 resolve 方法；
+ 异步函数内部使用 `throw` 关键字抛出错误，抛出后的代码不再执行
+ 调用异步函数再链式调用 then 方法获取异步函数执行结果
+ 调用异步函数再链式调用 catch 方法获取异步函数执行的错误信息



**await关键字**

+ 只能出现在异步函数中
+ await promise await 后面只能写promise对象，写其他类型的API不可以
+ await 可以暂停异步函数的执行，等待promise对象返回结果后再向下执行

