---
title: PHP第一季（十五）  MySQL数据库
date: 2017-04-15 14:56:04
tags: [PHP,李炎恢]
categories: 后台
---
## Web数据库概述
### 数据库的优点：

+ 关系型数据库比普通文件的数据访问速度更快
+ 关系型数据库更容易查阅并提取满足特定条件的数据
+ 关系型数据库更具有专门的内置机制处理并发访问，作为程序员，不需要为此担心
+ 关系型数据库可以提供对数据的随即访问
+ 关系型数据库具有内置的权限系统

### 关系数据库的概念
>至今为止，关系数据库是最常用的数据库类型。在关系代数方面，它们具有很好的理论基础。当使用关系数据库的时候，并不需要了解关系理论（这是一件好事），但是还是需要理解一些关于数据库的基本概念。

#### 表格
关系数据库由关系组成，这些关系通常称为表格。
一个关系就是一个数据的表格。
电子数据表就是一种表格。

|Player|Number|Team|
|:---:|:---:|:---:|
|JLin|7|Nets|
|Scurry|30|Warriors|
|CPaul|3|Clippers|

#### 列
表中的每一列都有惟一的名称，包含不同的数据。
每一列都有一个相关的数据类型。

#### 行
表中的每一行代表一个客户。
每一行具有相同的格式，因而也具有相同的属性。
行也成为记录。

#### 值
每一行由对应每一列的单个值组成。
每个值必须与该列定义的数据类型相同。

#### 键
每一条数据所对应的唯一的标识。

#### 模式
数据库整套表格的完整设计成为数据库的模式。

#### 关系
外键标识两个表格数据的关系。

### 如何设计Web数据库
+ 考虑要建模的实际对象
+ 避免保存冗余数据
+ 使用原子列值（对每一行的每个属性只存储一个数据。）
+ 选择有意义的键
+ 考虑需要询问数据库的问题
+ 避免多个空属性的设计

### Web数据库架构
浏览器和Web服务器之间的通信：
![浏览器和Web服务器之间的通信](http://blogpic.at15cm.com/php15-2.png)

浏览器和PHP&MySQL服务器之间的通信:
![浏览器和PHP&MySQL服务器之间的通信](http://blogpic.at15cm.com/php15-1.png)

1. 用户的Web浏览器发出HTTP请求，请求特定Web页面。
2. Web服务器收到.php的请求获取该文件，并将它传到PHP引擎，要求它处理。
3. PHP引擎开始解析脚本。脚本中有一条连接数据库的命令，还有执行一个查询的命令。PHP打开通向MYSQL数据库的连接，发送适当的查询。
4. MYSQL服务器接收数据库查询并处理。将结果返回到PHP引擎。
5. PHP以你去哪干完成脚本运行，通常，这包括将查询结果格式化成HTML格式。然后再输出HTML返回到Web服务器。
6. Web服务器将HTML发送到浏览器。


## MySQL操作
### 登录到MySQL
1. 打开MySQL Command Line Client
2. 输入root的设置密码

### MySQL常规命令
#### 1.显示当前数据库的版本号和日期。
```
SELECT VERSION(),CURRENT_DATE();
```

#### 2.通过AS关键字设置字段名。
```
SELECT VERSION() AS version;  //可设置中文，通过单引号
```

#### 3.通过SELECT 执行返回计算结果
```
SELECT (20+5)*4;
```

#### 4.通过多行实现数据库的使用者和日期
```
>SELECT
>USER()
>,
>NOW()
>;
```

#### 5.通过一行显示数据库使用者和日期
```
>SELECT USER();SELECT NOW();
```

#### 6.命令的取消
```
>\c
```

#### 7.MySQL窗口的退出
```
>exit;
```


### MySQL常用数据类型
整数型：TINYINT，SMALLINT，INT，BIGINT
浮点型：FLOAT，DOUBLE，DECIMAL(M,D)
字符型：CHAR，VARCHAR
日期型：DATETIME，DATE，TIMESTAMP
备注型：TINYTEXT，TEXT，LONGTEXT

**日期型**

|列类型|“零”值|
|:---|:---|
|DATETIME|'0000-00-00 00:00:00'|
|DATE|'0000-00-00'|
|TIMESTAMP|00000000000000|
|TIME|'00:00:00'|
|YEAR|0000|


**字符串型**

|值|CHAR(4)|存储需求|VARCHAR(4)|存储需求|
|:---:|:---:|:---:|:---:|:---:|
|''|'&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'|4个字节|''|1个字节|
|'ab'|'ab&nbsp;&nbsp;&nbsp;&nbsp;'|4个字节|'ab '|3个字节|
|'abcd'|'abcd'|4个字节|'abcd'|5个字节|
|'abcdefgh'|'abcd'|4个字节|'abcd'|5个字节|

'ab&nbsp;&nbsp;&nbsp;&nbsp;'

+ 采用CHAR类型，空格也是一个字符 (CHAR定长类型)，一般用于性别、密码，访问速度快；
+ VARCHAR会把口面的空格删除，自身长度加1(VCHAR可变长度的类型) ，一般用于用户名，文章标题；


**整数型**

|类型|字节|最小值|最大值|
|:---|:---|:---|:---|
|||(带符号的/无符号的)|(带符号的/无符号的)|
|TINYINT|1|-128|127|
|||0|255|
|SMALLINT|2|-32768|32767|
|||0|65535|
|MEDIUMINT|3|-8388608|8388607|
|||0|16777215|
|INT|4|-2147483648|2147483647|
|||0|4294967295|
|BIGINT|8|-9223372036854775808|9223372036854775807|
|||0|18446744073709551615|


**整数型**

|类型|字节|最小值|最大值|
|:---|:---|:---|:---|
|FLOAT|4|+-1.175494351E-38|+-3.402823466E+38|
|DOUBLE|8|+-2.2250738585072014E-308|+-1.7976931348623157E+308|
|DECIMAL|可变|它的取值范围可变|


**备注型**

|类型|描述|
|:---|:---|
|TINYTEXT|字符串，最大长度255个字符|
|TEXT|字符串，最大长度65535个字符|
|MEDIUMTEXT|字符串，最大长度16777215个字符|
|LONGTEXT|字符串，最大长度4294967295个字符|

备注型：string+1
常用的是TEXT,用于备注，大文章，帖子，新闻内容

### MySQL数据库操作
#### 1.显示当前存在的数据库
```
>SHOW DATABASES;
```

#### 2.选择你所需要的数据库
```
>USE guest;
```

#### 3.查看当前所选择的数据库
```
>SELECT DATABASE();	
```

#### 4.查看一张表的所有内容
```
>SELECT * FROM guest;   //可以先通过SHOW TABLES;来查看有多少张表
```

#### 5.根据数据库设置中文编码
```
>SET NAMES gbk;   //set names utf8;
```

#### 6.创建一个数据库
```
>CREATE DATABASE book;
```

#### 7.在数据库里创建一张表
```
>CREATE TABLE users (
>username VARCHAR(20),   //NOT NULL 设置不允许为空
>sex CHAR(1),
>birth DATETIME);
```

#### 8.显示表的结构
```
>DESCIRBE users;
```

#### 9.给表插入一条数据
```
>INSERT INTO users (username,sex,birth) VALUES ('Lee','x',NOW());
```

#### 10.筛选指定的数据
```
> SELECT * FROM users WHERE username = 'Lee';
```

#### 11.修改指定的数据
```
>UPDATE users SET sex = '男' WHERE username='Lee';
```

#### 12.删除指定的数据
```
> DELETE FROM users WHERE username='Lee';
```

#### 13.按指定的数据排序
```
> SELECT * FROM users ORDER BY birth DESC;
```

#### 14.删除指定的表
```
>DROP TABLE users;
```

#### 15.删除指定的数据库
```
>DROP DATABASE book;
```


## MySQL常用函数
### 文本函数

|函数|用法|描述|
|:---:|:---:|:---|
|CONCAT()|CONCAT(x,y,...)|创建形如xy的新字符串|
|LENGTH()|LENGTH(column)|返回列中储存的值的长度|
|LEFT()|LEFT(column,x)|从列的值中返回最左边的x个字符|
|RIGHT()|RIGHT(column,x)|从列的值中返回最右边的x个字符|
|TRIM()|TRIM(column)|从存储的值删除开头和结尾的空格|
|UPPER()|UPPER(column)|把存储的字符串全部大写|
|LOWER()|LOWER(column)|把存储的字符串全部小写|
|SUBSTRING()|SUBSTRING(column, start, length)||从column中返回开始start的length个字符（索引从0开始）|
|MD5()|MD5(column)|把储存的字符串用MD5加密|
|SHA()|SHA(column)|把存储的字符串用SHA加密|


### 数字函数

|函数|用法|描述|
|:---:|:---:|:---|
|ABS()|ABS(x)|返回x的绝对值|
|CEILING()|CEILING(x)|返回x的值的最大整数|
|FLOOR()|FLOOR(x)|返回x的整数|
|ROUND()|ROUND(x)|返回x的四舍五入整数|
|MOD()|MOD(x)|返回x的余数|
|RNAD()|RNAD()|返回0-1.0之间随机数|
|FORMAT()|FORMAT(x,y)|返回一个格式化后的小数|
|SIGN()|SIGN(x)|返回一个值，正数(+1)，0，负数(-1)|
|SQRT()|SQRT(x)|返回x的平方根|


### 日期和时间函数

|函数|用法|描述|
|:---:|:---:|:---|
|HOUR()|HOUR(column)|只返回储存日期的小时值|
|MINUTE()|MINUTE(column)|只返回储存日期的分钟值|
|SECOND()|SECOND(column)|只返回储存日期的秒值|
|DAYNAME()|DAYNAME(column)|返回日期值中天的名称|
|DAYOFMONTH()|DAYOFMONTH(column)|返回日期值中当月第几天|
|MONTHNAME()|MONTHNAME(column)|返回日期值中月份的名称|
|MONTH()|MONTH(column)|返回日期值中月份的数字值|
|YEAR()|YEAR(column)|返回日期值中年份的数字值|
|CURDATE()|CURDATE()|返回当前日期|
|CURTIME()|CURTIME()|返回当前时间|
|NOW()|NOW()|返回当前时间和日期|


### 格式化日期和时间
DATE_FORMAT()和TIME_FORMAT()

|名词|用法|示例|
|:---:|:---:|:---|
|%e|一月中的某天|1~31|
|%d|一月中的某天，两位|01~31|
|%D|带后缀的天|1st~31st|
|%W|周日名称|Sunday~Saturday|
|%a|简写的周日名称|Sun-Sat|
|%c|月份编号|1~12|
|%m|月份编号，两位|01~12|
|%M|月份名称|January~December|
|%b|简写的月份名称|Jan~Dec|
|%Y|年份|2002|
|%y|年份，两位|02|
|%l|小时|1~12|
|%h|小时,两位|01~12|
|%k|小时，24小时制|0~23|
|%H|小时，24小制度，两位|00~23|
|%i|分钟|00~59|
|%S|秒|00~59|
|%r|时间|8:17:02 PM|
|%T|时间，24小时制|20:17:02 PM|
|%p|上午或下午|AM或PM|


## SQL语句
### SQL语句详解
1. 创建一个班级数据库school，里面包含一张班级表grade，包含编号(id)、姓名(name)、邮件(email)、评分(point)、注册日期(regdate)。
```
mysql>CREATE DATABASE school;   //创建一个数据库
mysql> CREATE TABLE grade (
//UNSIGNED表示无符号，TINYINT(2) 无符号整数0-99，NOT NULL表示不能为空，AUTO_INCREMENT表示从1开始没增加一个字段，累计一位
    -> id TINYINT(2) UNSIGNED NOT NULL AUTO_INCREMENT,
    -> name VARCHAR(20) NOT NULL,
    -> email VARCHAR(40),
    -> point TINYINT(3) UNSIGNED NOT NULL,
    -> regdate DATETIME NOT NULL,
    -> PRIMARY KEY (id)   //表示id为主键，让id值唯一，不得重复。
    -> );
```

2. 给这个班级表 grade新增5-10条学员记录
```
mysql> INSERT INTO grade (name,email,point,regdate) VALUES 
('JLin','JLin7@gmail.com',95,NOW());
```

3. 查看班级所有字段的记录，查看班级id,name,email的记录
```
mysql> SELECT * FROM grade;
mysql> SELECT id,name,email FROM grade;
```

**WHERE表达式的常用运算符**

|MYSQL运算符|含义|
|:---:|:---:|
|=|等于|
|<|小于|
|>|大于|
|<=|小于或等于|
|>=|大于或等于|
|!=|不等于|
|IS NOT NULL|具有一个值|
|IS NULL|没有值|
|BETWEEN|在范围内|
|NOT BETWEEN|不在范围内|
|IN|指定的范围|
|OR|两个条件语句之一为真|
|AND|两个条件语句都为真|
|NOT|条件语句不为真|

4. 姓名等于'Lee'的学员，成绩大于90分的学员，邮件不为空的成员，70-90之间的成员
```
mysql> SELECT * FROM grade WHERE name='Lee';
mysql> SELECT * FROM grade WHERE point>90;
mysql> SELECT * FROM grade WHERE email IS NOT NULL;
mysql> SELECT * FROM grade WHERE point BETWEEN 70 AND 90;
mysql> SELECT * FROM grade WHERE point IN (95,82,78);
```

5. 查找邮件使用163的学员，不包含JLin7字符串的学员
```
mysql> SELECT * FROM grade WHERE email LIKE '%163.com';
mysql> SELECT * FROM grade WHERE email NOT LIKE '%JLin7%';
```

6. 按照学员注册日期的倒序排序，按照分数的正序排序
```
mysql> SELECT * FROM grade ORDER BY regdate DESC;
mysql> SELECT * FROM grade ORDER BY point ASC;
```

7. 只显示前三条学员的数据，从第3条数据开始显示2条
```
mysql> SELECT * FROM grade LIMIT 3;
mysql> SELECT * FROM grade LIMIT 2,2;
```

8. 修改姓名为'Lee'的电子邮件
```
mysql> UPDATE grade SET email='yc60.com@163.com' WHERE name='Lee';
```

9. 删除编号为4的学员数据
```
mysql> DELETE FROM grade WHERE id=4;
```

**MYSQL分组函数**

|函数|用法|描述|
|:---:|:---:|:---|
|AVG()|AVG(column)|返回列的平均值|
|COUNT()|COUNT(column)|统计行数|
|MAX()|MAX(column)|求列中的最大值|
|MIN()|MIN(column)|求列中的最小值|
|SUM()|SUM(column)|求列中的和|

```
mysql> SELECT AVG(point) FROM grade;
```

10. 检查这个表的信息
```
mysql> SHOW TABLE STATUS \G;
```

11. 优化一张表
```
mysql> OPTIMIZE TABLE grade;
```


### PhpMyAdmin
phpMyAdmin(简称PMA)是一个用PHP编写的，可以通过互联网在线控制和操作MySQL。他是众多MySQL管理员和网站管理员的首选数据库维护工具，通过phpMyAdmin可以完全对MySQL数据库进行操作。

#### 创建数据库school
创建一个数据库->选择utf8字符集

#### 导出另一个数据库SQL
选择另一个数据库->导出
选择需要导出的表->全选
选择Add DROP TABLE / DROP VIEW （基本表一旦删除，表中的数据以及相应建立的索引和视图都将自动被删除）
选择另存为文件
选择执行，保存sql文件

#### 导入数据库
选择被导入的数据库
选择Import(导入)，选择sql文件
执行即可

#### 删除表
可以直接选择操作中的，然后确认即可删除数据表.
也可以选择复选按钮,然后选择选中项：，选择删除，执行即可

#### 重建表
找到sql文件中的刚才输出的建表语句.
复制建表语句
然后选择sql，选择粘贴，执行即可

#### 修复数据表
选择要修复的表
在选中项中，选择修复表,即可

#### 优化数据表
选择要优化的表
在选中项中，选择优化表，即可

#### 修改，删除，插入表记录

#### 执行SQL语句

