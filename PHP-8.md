---
title: PHP第一季（八） 字符串处理
date: 2017-04-05 22:21:18
tags: [PHP,李炎恢]
categories: 后台
---
>在每天的编程工作中，处理、调整以至最后控制字符串是很重要的一部分，一般也认为这是所有编程语言的基础。不同与其他语言，PHP没有那么麻烦地使用数据类型来处理字符串。这样一来，PHP中的字符串处理就再容易不过了。

## 字符串格式化
### 清除空格
整理字符串的第一步是清理字符串中多余的空格。虽然这一部操作不是必需的，但如果要将字符串存入一个文件或数据库中，或者将它和别的字符串进行比较，这就是非常有用的。

+ chop()函数:移除字符串后面多余的空白，包括新行
+ ltrim()函数:移除字符串起始处多余空白
+ rtrim()函数:移除字符串后面多余的空白，包括新行，此函数是chop()的别名
+ trim()函数:移除字符串两边多余的空白

```
<?php
	echo trim('              JLin7           ');
?>
```

PHP具有一系列可供使用的函数来重新格式化字符串:

+ nl2br()函数
将字符串作为输入参数，用XHTML中的<br />标记代替字符串中的换行符。

```
<?php
	echo nl2br("I can do all things\nsaid Stephen Curry");
?>
```

+ htmlentities()函数
将字符转换为 HTML 转义字符

+ htmlspecialchars()函数
将特殊字符转换为 HTML 实体

+ strip_tags()
从字符串中去除 HTML 和 PHP 标记

```
<?php
	echo htmlentities('<strong>JLin7</strong>');  //转换所有字符
	echo htmlspecialchars('<strong>JLin7</strong>')  //转换特殊字符
	echo strip_tags('<strong>JLin7</strong>')  //去掉了<strong>
?>
```


>对于字符串来说，某些字符肯定是有效的，但是当将数据插入到数据库中的时候可能会引起一些问题，因为数据库会将这些字符解释成控制符。这些有问题的字符就是引号（单引号和双引）、反斜杠（\）和NULL字符。

+ addslashes()
使用反斜线引用字符串
将任何字符串写到数据库之前，应该使用这个函数将它们重新格式化
在调用了addslashes()后，所有的引号都加了斜杠

+ stripslashes()函数
反引用一个引用字符串,去掉这些斜杠

```
<?php
	echo addslashes('I can do \a" all things! ');
?>
```

### 字母大小写
重新格式化字符串中的字母大小写

+ strtoupper()函数将字符串转换为大写
+ strtolower()函数将字符串转换成小写
+ ucfirst()函数将第一个字母转换为大写
+ ucwords()函数将每个单词第一个字母转换为大写

```
<?php
	echo strtoupper('I can do all things');
?>
```

### 填充字符串函数
+ str_pad()
将字符串用指定个数的字符填充字符串。

```
<?php
	echo str_pad('Salad',10).'is good.';
?>
```


### 操作子字符串
>通常，我们想查看字符串的各个部分。例如，查看句子中的单词，或者将一个域名或电子邮件地址分割成一个个的组件部分。PHP提供了几个字符串函数来实现此功能。

#### explode()、implode()和join()
+ explode()
使用一个字符串分割另一个字符串

+ implode()
将一个一维数组的值转化为字符串

+ join()
和implode()函数效果一样

```
<?php
	$email = 'legendaryarthur@163.com';
	$email_array  = explode('@',$email);
?>
```

#### strtok()函数
strtok()函数：
标记分割字符串，一次只从字符串取出一些片段（称为令牌）
对于一次从字符串中取出一个单词的处理来说，strtok()函数比explode()函数的效果更好。

```
<?php
	$str = "I,will.be#back";
	$tok = strtok($str,",.#");
	while($tok) {
    	echo "$tok<br \>";
   		$tok = strtok(",.#");
   }
?>
```

#### 使用substr()函数
函数substr()允许我们访问一个字符串给定起点和终点的子字符串。这个函数并不适用于我们的例子中，但是，当需要得到某个固定格式字符串中的一部分时，它会非常有用。

```
<?php
echo substr("abcdef", 1, 3);  
?>
```

>注意仅第一次调用 strtok 函数时使用 string 参数。后来每次调用 strtok，都将只使用 token 参数，因为它会记住它在字符串 string 中的位置。如果要重新开始分割一个新的字符串，你需要再次使用 string 来调用 strtok 函数，以便完成初始化工作。注意可以在 token 参数中使用多个字符。字符串将被该参数中任何一个字符分割。 

#### 分解字符串
str_split()
将字符串转换为数组，返回一个数组，其中各数组元素分别是字符串参数中的一个字符串。

```
<?php
	print_r(str_split('I can do all things'));
?>
```

#### 逆置字符串
strrev()可以将一个字符串逆反过来。

```
<?php
	echo strrev('I can do all things');
?>
```

## 字符串比较
到目前为止，我们已经用过"=="号来比较两个字符串是否相等。使用PHP可以进行一些更复杂的比较。这些比较分为两类：部分匹配和其他情况。

### 字符串的排序
+ strcmp()
二进制安全字符串比较,该比较区分大小写

+ strcasecmp()
二进制安全比较字符串（不区分大小写）

+ strnatcmp()
使用自然排序算法比较字符串,该函数实现了以人类习惯对数字型字符串进行排序的比较算法，这就是“自然顺序”。注意该比较区分大小写。 

该函数需要两个进行比较的参数字符串。如果这两个字符串相等，该函数返回0，如果按字典顺序str1和str2后面（大于str2）就返回一个正数，如果str1小于str2就返回一个负数。这个函数是区分大小写的。

```
<?php
	echo strcmp('a','b');  
?>
```

+ strspn()函数
返回一个字符串中包含有另一个字符串中字符的第一部分的长度。也就是求两个字符串之间相同的部分。

```
<?php
	echo strspn('all','I can do all things');
?>
```

+ strlen()函数
测试字符串的长度
可以使用函数strlen()来检查字符串的长度。如果传给它一个字符串，这个函数将返回字符串的长度。例如, strlen("hello")将返回5。

```
<?php
	echo strlen('I can do all things');
?>
```

+ substr_count()
确定字符串出现的频率,返回一个字符串在另一个字符串中出现的次数。

```
<?php
	echo substr_count('I can do all things','c');
?>
```

### 查找替换字符串
通常，我们需要检查一个更长的字符串中是否含有一个特定的子字符串。这种部分匹配通常比测试字符串的完全等价更有用处。
在字符串中查找字符串：strstr()、strchr()、strrchr()和stristr()

+ strstr()
用于在一个较长的字符串专供查找匹配的字符串或字符(查找字符串的首次出现)

+ strchr()
函数strchr()和strstr()完全一样。

```
<?php
	echo strstr('I can do @ all things','@');
?>
```

函数strstr()有两个变体:

+ stristr()
它几乎和strstr()一样，其区别在于不区分字符大小
对于我们的只能表单应用程序来说，这个函数非常有用，因为用户可以输入"delivery"、"Delivery"和"DELIVERY"。

+ strrchr()
它也几乎和strstr()一样，只不过是strstr()的别名。

### 查找字符串的位置
+ strpos()
查找字符串首次出现的位置

+strrpos()
计算指定字符串在目标字符串中最后一次出现的位置

>函数strpos()和strrpos()的操作和strstr()类似，但它不是返回一个子字符串，而返回子字符串needle在字符串haystack中的位置。更有趣的是，现在的PHP手册建议使用strpos()函数代替strstr()函数来查看一个子字符串在一个字符串中出现的位置，因为前者的运行速度更快。

```
<?php
	echo strrpos('i can do all things','c');
?>
```

### 替换字符串
+ str_replace()
子字符串替换

+ str_ireplace()
str_replace() 的忽略大小写版本

+ substr_replace()
替换字符串的子串

```
<?php
	// 赋值: <body text='black'>
	$bodytag = str_replace("%body%", "black", "<body text='%body%'>")
?>
```

```
$var = 'ABCDEFGH:/MNRPQR/';
/* 这两个例子使用 “bob” 替换整个 $var。*/
echo substr_replace($var, 'bob', 0) . "<br />\n";
echo substr_replace($var, 'bob', 0, strlen($var)) . "<br />\n";
/* 将 “bob” 插入到 $var 的开头处。*/
echo substr_replace($var, 'bob', 0, 0) . "<br />\n";
/* 下面两个例子使用 “bob” 替换 $var 中的 “MNRPQR”。*/
echo substr_replace($var, 'bob', 10, -1) . "<br />\n";
echo substr_replace($var, 'bob', -7, -1) . "<br />\n";
/* 从 $var 中删除 “MNRPQR”。*/
echo substr_replace($var, '', 10, -1) . "<br />\n";
```

## 处理中文字符
>对于以上的字符串函数，有些可以用于中文，但有些却不适用中文。所以，PHP提供了专门的函数来解决这样的问题。

中文字符可以是gbk,utf8,gb2312

+ mb_strlen() 
求字符串的长度
对应的函数为 strlen()  

+ mb_strstr()  
求某字符串到结尾的字符
对应的函数为 strstr()

+ mb_strpos()
求出字符最先出现处 
对应的函数为 strpos()  

+ mb_substr() 
取出指定的字符串
对应的函数为 substr()
  
+ mb_substr_count() 
返回字符串出现的次数
对应函数为 substr_str() 

