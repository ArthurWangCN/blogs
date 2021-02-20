---
title: PHP第一季（十二） 会话控制
date: 2017-04-08 21:19:47
tags: [PHP,李炎恢]
categories: 后台
---
>HTTP（超文本传输协议）定义了通过万维网（WWW）传输文本、图形、视频和所有其他数据所有的规则。HTTP是一种无状态的协议，说明每次请求的处理都与之前或之后的请求无关。虽然这种简化实现对于HTTP的普及做出了卓越的贡献，但对于希望创建复杂的Web应用程序的开发人员来说，这点有点困扰。为了解决这个问题，出现了一种在客户端机器上存储少量信息（cookie）。由于cookie大小限制、数量及其他原因，开发人员又提出了一种解决方案：session会话处理。

## Cookie的应用
### 设置cookie
setcookie()函数可以在客户端生成一个cookie文件，这个文件可以保存到期时间、名称、值等。
创建cookie
```
<?php
	setcookie('name','Lee',time()+(7*24*60*60)); 
	//参数1：cookie名称
	//参数2：cookie值
	//参数3：cookie过期时间
?>
```

### 读取cookie
```
<?php
	echo $_COOKIE['name'];
?>
```

### 删除cookie
```
<?php
	setcookie('name','');
	setcookie('name','Lee',time()-1);
?>
```

**使用Cookie的限制**
1、必须在HTML文件的内容输出之前设置；
2、不同的浏览器对Cookie的处理不一致，且有时会出现错误的结果。
3、限制是在客户端的。一个浏览器能创建的Cookie数量最多为30个，并且每个不能超过4KB，每个WEB站点能设置的Cookie总数不能超过20个。

## Session会话处理
在使用session会话处理，必须开始session，使用session_start()开始会话。

### 创建session并读取session
```
<?php
	session_start();
	$_SESSION['name'] = 'Lee';
	echo $_SESSION['name'];
?>
```

### 判断session是否存在
```
<?php
	session_start();
	$_SESSION['name'] = 'Lee';
	if (isset($_SESSION['name'])) {
		echo $_SESSION['name'];
	}
?>
```

### 删除session
```
<?php
	session_start();
	$_SESSION['name'] = 'Lee';
	unset($_SESSION['name']);
	echo $_SESSION['name'];
?>
```

### 销毁所有session
```
<?php
	session_start();
	$_SESSION['name'] = 'Lee';
	$_SESSION['name2'] = 'Lee';
	session_destroy();
	echo $_SESSION['name'];
	echo $_SESSION['name2'];
?>
```