---
title:  Mac OS自带apache基本用法总结
date: 2017-02-24 09:01:51
tags: Apache
categories: 后台
---

### 找到apache藏身之所
系统默认是隐藏 apache 安装目录的，但我们可以通过“命令行”或者“文件夹前往”的方式找到它。它是安装在系统的私有目录下，也就是 `/private/etc` 下面，因为它是隐藏的，所以我们无法通过界面找到它。
ps：/ect目录是mac os 系统存放系统配置信息的地方，里面都是xxx.conf的配置文件。

**命令行**
在终端输入 `open /etc` 命令即可打开它:
![终端打开](http://blogpic.at15cm.com/apache1.jpeg)

**文件夹前往**
打开 `Finder > 前往 ＞前往文件夹` 或快捷键 `command+shift+G`:
![文件夹前往](http://blogpic.at15cm.com/apache2.jpeg)

### apache下部署web资源
跟windows不一样，它的部署包不是放在htdocs(windows下的存放目录),而是放在 `/Library/WebServer/Documents` 或 `/资源库/WebServer/Documents/`下：
![部署包位置](http://blogpic.at15cm.com/apache3.jpeg)
那么我们的静态资源就可以丢到这个目录下去了。
这个目录是apache的默认目录，有时候为了方便操作，可能需要指向特定的文件夹，该如何修改apache的配置呢？

### 修改默认部署路径
找到下面 `httpd.conf` 文件，配置转发，模块启动停用之类操作都在该文件里面。
![httpd.conf](http://blogpic.at15cm.com/apache4.jpeg)
找到这个DocumentRoot，修改成你想要的地址即可 
![httpd.conf](http://blogpic.at15cm.com/apache5.jpeg)

### 启动停用apache
开启apache:  `sudo apachectl start`
重启apache:  `sudo apachectl restart`
关闭apache:  `sudo apachectl stop`
如果需要password，输入即可，如果启动失败了，就可以去看apache的日志，找到错误的原因（前提是在httpd.conf中配置了日志的路径）
![启动成功](http://blogpic.at15cm.com/apache6.jpeg)
在浏览器中输入localhost或者127.0.0.1即可看到“It works!”的提示。恭喜，apache启动成功了。验证:<http://127.0.0.1>

### 修改apache默认端口
通过localhost或者127.0.0.1访问，表示默认的端口是 `80`，有时候如果80端口被占用了，就得换个端口试试了。同样是在httpd.conf下面，找到Listen 80 那一行，修改成你想要的端口即可。
![默认端口](http://blogpic.at15cm.com/apache7.jpeg)

![修改默认端口](http://blogpic.at15cm.com/apache8.jpeg)


参考文献：
+ [Mac OS原来自带了apache，基本用法总结](http://blog.csdn.net/seafishyls/article/details/44546809)
+ [Mac自带的本地服务器的使用](http://www.jianshu.com/p/90d5fa728861)


