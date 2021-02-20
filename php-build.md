---
title: 如何在Macbook Pro搭建PHP开发环境
date: 2017-03-28 17:13:42
tags: PHP
categories: 后台
---
## 启动Apache
### 常用命令
+ 启动Apache服务
sudo apachectl start

+ 重启Apache服务
sudo apachectl restart

+ 停止Apache服务
sudo apachectl stop

+ 查看Apache版本
httpd -v

### 操作
1. Mac OS自带Apache，只需要启动Apache就行。
打开终端，输入命令：`sudo apachectl start`

2. 打开浏览器，在地址栏中输入 `localhost`，出现 `It Works` 字符串，就说明Apache已经成功启动

3. 在Macbook pro下，Apache的网站服务器根目录在 `/Library/WebServer/Documents` 路径下

>更多：[Mac OS自带apache基本用法总结](http://legendaryarthur.cn/2017/02/24/mac-apache/)

## 配置PHP
+ Mac OS 同样自带PHP，只需要在Apache的配置文件中添加Apache对PHP的支持就好了

在终端中输入命令打开 `httpd.conf` 文件:
```
sudo vim /etc/apache2/httpd.conf
```

+ 去掉红框标注内容的注释符号然后保存
```
LoadModule php5_module libexec/apache2/libphp5.so
```
![配置PHP](http://blogpic.at15cm.com/httpd.png)
[mac下使用自带的vim编辑器编辑文件](http://legendaryarthur.cn/2017/03/28/mac-vim-edit/)
>如果不会用vim完全可以找到 `httpd.conf` 文件用你熟悉的编辑器来编辑

+ 重启Apache服务
在终端输入 `sudo apachectl restart`
大功告成，去测试一下吧




