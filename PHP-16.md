---
title: PHP第一季（十六）  PHP操作MySQL
date: 2017-04-16 16:17:36
tags: [PHP,李炎恢]
categories: 后台
---
## PHP连接到MySQL
### 编码
全面采用UTF-8编码
设置Zend Stduio的编码：Window -> Preferences -> Workspace

### 标头设置
让火狐和IE保持编码统一：
```
<?php
	header('Content-Type:text/html; charset=utf-8');
?>
```

### 连接MySQL
mysql_connect — 打开一个到 MySQL 服务器的连接
```
<?php
	mysql_connect('localhost','root','123456');
	//第一个参数为MySQL服务器地址，默认3306端口
	//第二个参数为服务器用户名
	//第三个参数为服务器密码
?>
```

```
<?php
	$conn = @mysql_connect(DB_HOST,DB_USER,DB_PASSWORD) or die('数据库连接失败！错误信息：'.mysql_error());
	//@如果出错了，不要出现警告或错误，直接忽略掉
	//die函数之前，先连接一下，报错流程
?>
```

>这里可能报告”No such file or directory”错误，查看 [解决方法](http://legendaryarthur.cn/2017/04/16/PHP-16-bug1/)。


数据库连接参数，可以用常量存储，这样就不能修改，更加安全。
```
<?php
	define('DB_USER','root');
	define('DB_PASSWORD','123456');
	define('DB_HOST','localhost');
	define('DB_NAME','school');
?>
```

### 选择你所需要的数据库
```
<?php
	@mysql_select_db(DB_NAME) or die('数据库找不到！错误信息：'.mysql_error());
?>
```

设置字符集，如果是GBK，直接设置SET NAMES GBK即可
```
<?php
	@mysql_query('SET NAMES UTF8') or die('字符集设置错误');
?>
```

### 获取记录集
从这个数据库里选一张表（grade），然后把这张表的数据库提出来

```
<?php
	$query = "SELECT * FROM grade";
	$result = @mysql_query($query) or die('SQL语句有误！错误信息：'.mysql_error());
	//$result就是记录集
?>
```

### 输出一条记录
```
<?php
	print_r(mysql_fetch_array($result,MYSQL_ASSOC));
?>
```

### 释放结果集资源
```
<?php
	mysql_free_result($result);
?>
```

### 关闭数据库
```
<?php
	mysql_close($conn);
?>
```


## 增删改查
### 新增数据
```
<?php
	$query = "INSERT INTO grade (name,email,point,regdate) VALUE
 ('JLin','JLin7@gmail.com',,NOW())";
	@mysql_query($query) or die('添加数据出错：'.mysql_error());
?>
```

### 修改数据
```
<?php
	$query = "UPDATE grade SET name='LBJ' WHERE id=6";
	@mysql_query($query) or die('修改出错：'.mysql_error());
?>
```

### 删除数据
```
<?php
	$query = "DELETE FROM grade WHERE id=6";
	@mysql_query($query) or die('删除错误：'.mysql_error());
?>
```

### 显示数据
```
<?php
	$query = "SELECT id,name,email,point FROM grade";
	$result = @mysql_query($query) or die('查询语句出错：'.mysql_error());
	while (!!$row = mysql_fetch_array($result)) {
		echo $row['id'].'----'.$row['name'].'----'.$row['email'].'----'.$row['point'];
		echo '<br />';
	}
?>
```


## 其他常用函数

mysql_fetch_row()：从结果集中取得一行作为枚举数组
mysql_fetch_assoc()： 从结果集中取得一行作为关联数组 
mysql_fetch_array()： 从结果集中取得一行作为关联数组，或数字数组，或二者兼有 

mysql_fetch_lengths()： 取得结果集中每个输出的长度 
mysql_field_name()：  取得结果中指定字段的字段名 

mysql_num_rows()： 取得结果集中行的数目
mysql_num_fields()：取得结果集中字段的数目

mysql_get_client_info()： 取得 MySQL 客户端信息
mysql_get_host_info()： 取得 MySQL 主机信息
mysql_get_proto_info()： 取得 MySQL 协议信息
mysql_get_server_info()： 取得 MySQL 服务器信息
