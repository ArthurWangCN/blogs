---
title: PHP第一季（十） 日期和时间
date: 2017-04-07 22:09:37
tags: [PHP,李炎恢]
categories: 后台
---
>使用PHP编程时，与你遇到的大多数其他类型的数据相比，日期和时间有很大不同。因为日期和时间没有明确的结构，并且日期的计算和表示也很麻烦。在PHP中，日期和时间函数库是PHP语言的一个核心部分。

   时间戳是自 1970 年 1 月 1 日（00:00:00 GMT）以来的秒数。它也被称为 Unix 时间戳（Unix Timestamp）。Unix时间戳(Unix timestamp)，或称Unix时间(Unix time)、POSIX时间(POSIX time)，是一种时间表示方式，定义为从格林威治时间1970年01月01日00时00分00秒起至现在的总秒数。Unix时间戳不仅被使用在Unix 系统、类Unix系统中，也在许多其他操作系统中被广泛采用。例如(1184557366表示2007-07-16 03:42:46)
   
## PHP日期和时间库
### 验证日期
+ checkdate()函数
能够很好地验证日期，提供的日期如果有效，则返回true，否则返回false。
```
<?php
	if (checkdate(2,29,2007)) {
		echo '日期合法';
	} else {
		echo '日期不合法';
	}
?>
```

### 格式化时间和日期
+ date()函数
返回根据预定义指令格式化时间和日期的字符串形式。所有格式参数，可以参考手册。
```
<?php
	echo date('Y-m-d H:i:sa');  //直接输入日期和时间
echo date('今天的日期和时间为：Y/m/d H:i:sa');  //可以插入无关的字符串
?>
```

### 查看更多时间信息
+ gettimeofday()函数
返回与当前时间有关的元素所组成的一个关联数组。
```
<?php
	print_r(gettimeofday()); //可以传入一个真(1)
?>
```

### 将时间戳转换成友好的值
+ getdate()函数
接受一个时间戳，并返回一个由其各部分组成的关联数组。如果不给参数，那么返回当前的时间和日期。
```
<?php
	print_r(getdate(1184557366));
?>
```

### 获取当前的时间戳
+ time()函数
可以获取当前的时间戳，并且可以通过设置时间戳的值。
```
<?php
	echo date('Y-m-d H:i:s',time()+(7*24*60*60));
?>
```

### 获取特定的时间戳
+ mktime()函数
可以生成给定日期时间的时间戳。
```
<?php
	echo mktime(14,14,14,11,11,2007);		//1194761654
	echo date('Y-m-d H:i:s',mktime(14,14,14,11,11,2007));		//2007-11-11 14:14:14
?>
```

### 计算时间差
```
<?php
	$now = time();
	$taxday = mktime(0,0,0,7,17,2017);
	echo round(($taxday - $now)/60/60);
?>
```

### 将日期转换成时间戳
+ strtotime()
将人可读的日期转换为Unix时间戳。
```
<?php
	echo strtotime('2007-10-31 14:31:33');
?>
```

### 计算时间差
```
<?php
	echo (strtotime('2017-10-31 14:31:33') - strtotime('2017-10-31 11:31:33'))/60/60;
?>
```

### 获取当前文件最后修改时间
+ getlastmod()
可以得到当前文件最后修改时间的时间戳。
```
<?php
	echo date('Y-m-d H:i:s',getlastmod());
?>
```

### 设置时区和GMT/UTC：
修改php.ini文件中的设置，找到[date]下的;date.timezone = 选项，将该项修改为date.timezone=Asia/Shanghai，然后重新启动apache服务器。

+ putenv()函数可以设置当前的默认时区。
```
<?php
	putenv('TZ=Asia/Shanghai');
	echo date('Y-m-d H:i:s');
?>
```

date_default_timezone_set()可以设置当前的默认时区。
date_default_timezone_get()可以获取当前的默认时区。

```
<?php
date_default_timezone_set('Asia/Shanghai');
	echo date('Y-m-d H:i:s');
?>
```

### 取得本地时间
+ localtime()函数
取得本地时间数据，然后返回一个数组。
```
<?php
	date_default_timezone_set('Asia/Shanghai');
	print_r(localtime());
	print_r(localtime(time(), true));
?>
```

### 计算页面脚本运行时间
+ microtime()函数
该函数返回当前UNIX时间戳和微秒数。
返回格式为msec sec的字符串，其中sec是当前的UNIX时间戳，msec为微秒数。
```
<?php
	function fntime() {
		list($msec, $sec) = explode(' ', microtime());
		return $msec+$sec;
	}
	$start_time = fntime();
	for($i=0;$i<1000000;$i++) {
	 	//
	}
	$end_time = fntime();
	echo round($end_time - $start_time,4);
?>
```

