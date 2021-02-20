---
title: mysql_connect报告”No such file or directory”错误的解决方法
date: 2017-04-16 16:51:23
tags: PHP
categories: 后台
---
写了个php脚本单独执行mysql_connect()，发现错误信息是 `No such file or directory` ，mysql客户端可以正常使用，可以确定不是服务器的问题。

在网上搜了一下，原来，我的 apache/php 是 mac 系统自带的，而 mysql 是安装的，它的本地socket设置与默认的不一样，导致php无法找到mysql的socket文件。

**解决方法：**

1. 确定是 mysql_connect() 和 mysql_pconnect() 的问题，故障现象就是函数返回空，而 mysql_error() 返回 "No such file or directory"；
2. 写个 phpinfo 页面，找到 mysql.default_socket、mysqli.default_socket、pdo_mysql.default_socket； 
3. 启动 mysql，执行命令 `STATUS`; 记下 `UNIX socket` 的值；
4. 如果第2步和第3步的值不一样，则打开php.ini（可以从phpinfo页面中找到php.ini的位置，默认是/private/etc/php.ini），将第2步中提到的三个配置项的值改成3的值；
5. 重启apache。