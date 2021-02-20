---
title: Ajax基础
date: 2017-02-26 14:10:27
tags: [前端, ajax]
categories: 前端
---
## 什么是Ajax
`AJAX` : Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）
>AJAX 不是新的编程语言，而是一种使用现有标准的新方法。
AJAX 是与 `服务器` 交换数据并更新部分网页的艺术，在不重新加载整个页面的情况下

## 前提
Ajax是与服务器进行交换数据，所以这里要在电脑上安装服务器：
+ Mac OS: 系统自带apache ，[教程](http://legendaryarthur.cn/2017/02/24/mac-apache/)
+ Windows: 视频中blue大神推荐用 [Wamp](http://www.wampserver.com/)，[安装教程](http://jingyan.baidu.com/article/22fe7ced7ba5403003617f60.html)

#### 扩展：Ajax阻止缓存
浏览器缓存是通过URL进行的，当使用Ajax进行数据交换时，如果更改数据，由于缓存的缘故，会出现数据得不到更新的情况，这种情况在IE中尤为明显，阻止缓存的方法：URL实时改变。
```
ajax('a.txt?t='+new Date().getTime(),function(res){
	console.log(res);
},function(res){
	alert('oops...');
})
```
*解释：*getTime() 返回从 1970 年 1 月 1 日至今的毫秒数,由于每一毫秒时间的值都不一样，所以会导致URL不同

## HTTP请求方法
+ `GET` —— 用于获取数据（如：浏览帖子）
+ `POST` —— 用于上传数据（如：用户注册）

GET&POST:
与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。然而，在以下情况中，请使用 POST 请求：
1. 无法使用 `缓存文件`（更新服务器上的文件或数据库）
2. 向服务器发送 `大量数据`（POST 没有数据量限制）
3. 发送包含 `未知字符` 的用户输入时，POST 比 GET 更稳定也更可靠

## 编写Ajax
### 创建 XMLHttpRequest 对象
XMLHttpRequest 是 AJAX 的基础，所有现代浏览器均支持`XMLHttpRequest 对象`（IE5 和 IE6 使用 `ActiveXObject`）。
XMLHttpRequest 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
所有现代浏览器（IE7+、Firefox、Chrome、Safari 以及 Opera）均内建 XMLHttpRequest 对象。

**创建 XMLHttpRequest 对象的语法：**
```
variable=new XMLHttpRequest();
```

**老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象：**
```
variable=new ActiveXObject("Microsoft.XMLHTTP");
```

**扩展:**
`alert(a)` 结果：报错
`alert(window.a)` 结果：undefined
原因：直接alert一个没有定义的变量浏览器会报错，而 `window.a` 中的 a window下的一个属性，所以返回undefined。

### 异步 - True 或 False？
AJAX 指的是异步 JavaScript 和 XML（Asynchronous JavaScript and XML, XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true。
通过 AJAX，JavaScript 无需等待服务器的响应，而是：
+ 在等待服务器响应时执行其他脚本
+ 当响应就绪后对响应进行处理

### readyState
+ 0	（未初始化）还没有调用open()方法
+ 1	（载入）已调用send()方法，正在发送请求
+ 2	（载入完成）send()方法完成，已收到全部响应内容
+ 3	（解析）正在解析响应内容
+ 4	（完成）响应内容解析完成，可以在客户端调用了

### 封装
```
function ajax(url, fnSucc, fnFaild){
	//1.创建Ajax对象
	if(window.XMLHttpRequest){
		var oAjax=new XMLHttpRequest();
	}
	else{
		var oAjax=new ActiveXObject("Microsoft.XMLHTTP");
	}
	//2.连接服务器（打开和服务器的连接）
	oAjax.open('GET', url, true);	
	//3.发送
	oAjax.send();	
	//4.接收
	oAjax.onreadystatechange=function (){
		if(oAjax.readyState==4){
			if(oAjax.status==200){
				//alert('成功了：'+oAjax.responseText);
				fnSucc(oAjax.responseText);
			}
			else{
				if(fnFaild){
					fnFaild();
				}
			}
		}
	};
}
```

#### 参考文献
+ [AJAX 教程](http://www.w3school.com.cn/ajax/)
+ [智能社JavaScript视频教程-24.Ajax基础](http://bbs.zhinengshe.com/thread-1203-1-1.html)

