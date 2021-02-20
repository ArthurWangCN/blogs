---
title:  Mac 上配置phpMyadmin 
date: 2017-04-16 15:51:17
tags: PHP
categories: 后台
---
>phpMyAdmin(简称PMA)是一个用PHP编写的，可以通过互联网在线控制和操作MySQL。他是众多MySQL管理员和网站管理员的首选数据库维护工具，通过phpMyAdmin可以完全对MySQL数据库进行操作。

## 准备
首先要 [配置PHP环境](http://legendaryarthur.cn/2017/03/28/php-build/) 和 [配置MySQL环境](http://legendaryarthur.cn/2017/03/28/mysql-build/)

## 配置phpMyadmin 
要管理Mysql，如果用命令行比较麻烦，开源的phpMyAdmin采用C/S的模式，方便管理。接着我们就装一个phpMyAdmin，下载地址 [戳我](http://www.phpmyadmin.net/home_page/downloads.php) 。

将下载下来的解压放在 `/Library/WebServer/Documents/` 目录下，完整的目录为：/Library/WebServer/Documents/phpmyadmin/，那么命令行进入这个目录：
```
cd /Library/WebServer/Documents/phpmyadmin/
```

再输入命令：
```
cp config.sample.inc.php config.inc.php  
vim config.inc.php 
```

按照下面进行修改：
```
$cfg['blowfish_secret'] = '';//用于Cookie加密，随意的长字符串  
$cfg['Servers'][$i]['host'] = '127.0.0.1';//MySQL守护程序做了IP绑定  
```

现在可以在浏览器中输入URL: <http://localhost/phpmyadmin/>

![phpMyadmin](http://blogpic.at15cm.com/phpmyadmin.png)

用户名为:root
密码为你设置的密码。

就可以login到mysql的管理界面。