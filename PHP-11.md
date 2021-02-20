---
title: PHP第一季（十一） 表单与验证
date: 2017-04-07 23:35:22
tags: [PHP,李炎恢]
categories: 后台
---
>我们对Web感兴趣，认为它有用的原因是其主要通过基于HTML的表单发布和收集信息的能力。这些表单用来鼓励网站的反馈、进行论坛会话、收集在线定购的邮件地址，等等。但是对HTML表单进行编码只是有效接受用户输入的必须操作的一部分，必须由服务器端组件来处理输入。

## Header()函数
标头 (header) 是服务器以 HTTP 协议传 HTML 资料到浏览器前所送出的字符串，在标头与 HTML 文件之间尚需空一行分隔。

### 用于重新导向指定的URL
```
<?php
	header('Location:http://www.baidu.com');
?>
```

### 用于设置页面字符编码
```
<?php
	header('Content-Type: text/html; charset=gbk');
	echo '嘿嘿，我是中文！页面编码是GBK，文件也是GBK'; 
?>
```

注意：除非启用了输出缓冲，否则这些命令必须在返回任何输出之前执行。
启用输出缓冲：ob_start()
```
<?php
	ob_start();
?>
```

## 接受及验证数据
HTML表单元素

|表单元素|描述|
|:----:|:-----:|
|text input|文本框|
|passoword input|密码框|
|hidden input|隐藏框|
|select|下拉列表框|
|checkbox|复选框|
|radio|单选按钮|
|textarea|区域框|
|file|上传|
|submit|提交按钮|
|reset|重置按钮|

### GET与POST
处理表单时，必须指定输入到表单的信息以何种方式传输到其目的地（method=""）。
Web开发人员可以采用GET和POST。
使用GET方法发送数据时，所有域都追加到浏览器的URL后面，并且为数据随URL地址发送。
采用POST方法时，值会作为标准值发送。

PHP分别使用$_GET和$_POST超全局变量来处理GET和POST变量。
通过使用这两个超全局变量，可以准确地指定信息应当来自哪里，并以你希望的方式处理数据。

**使用$_GET或$_POST来接收数据**

+ $_GET['username']，发送的表单method必须是get；
+ $_POST['username']，发送的表单method必须是post；
+ 采用isset()来验证$_GET['username']超级全局变量是否定义；
+ 使用htmlspecialchars()函数将HTML特殊字符进行过滤。

```
if(isset($_POST['username'])){
	echo '正常提交';
} else {
	echo '非法提交';
	//这里的非法提交是没有经过表单提交，没有生成全局变量，而不是username字段为空
}
```
>注意：空字符串也是数据，也可以赋值给$_POST['username']


**对数据有效性进行验证**

+ 使用函数trim()去除数据的前后空格；
+ 使用函数strlen()判断数据的长度；
+ 使用函数is_numeric()判断数据是纯数字；
+ 使用正则表达式验证邮箱是否合法。

```
<?php
	if (!isset($_POST['send']) || $_POST['send']!='提交') {
		header('Location:Demo1.php');
		exit;
	}
	if (preg_match('/([\w\.]{2,255})@([\w\-]{1,255}).([a-z]{2,4})/',$_POST['email'])) {
		echo '电子邮件合法';
	} else {
		echo '电子邮件不合法';
	}
?>
```
