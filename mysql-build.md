---
title: 如何在Macbook上安装MySQL和MySQLWorkbench
date: 2017-03-28 17:40:23
tags: MySQL
categories: 后台
---
## 下载 MySQL 和 MySQL Workbench
选择合适自己Mac 版本的[MySQL安装包](http://dev.mysql.com/downloads/mysql/)(有很多中版本)

选择与上面下载的MySQL安装包一样的 [MySQL Workbench版本](http://dev.mysql.com/downloads/workbench/)（不过这里只有一个版本，所以就下这个）
![下载MySQL安装包](http://blogpic.at15cm.com/mysql-download.png)

## 安装MySQL
打开DMG安装包，双击安装包里的PKG文件，一路点击“继续”，最后点击“完成”
![安装MySQL](http://blogpic.at15cm.com/mysql-install.png)

>注意：安装完成之后会弹出一个对话框，告诉我们生成了一个root账户的临时密码。请注意保存，否则重设密码会比较麻烦。
![临时密码](http://blogpic.at15cm.com/mysql-password.png)

## 启动MySQL
安装完成后，打开“系统偏好设置”，可以看到多了一个MySQL图标，点击“MySQL”图标。
![启动MySQL](http://blogpic.at15cm.com/mysql-run.png)

安装之后，默认MySQL的状态是stopped，关闭的，需要点击“Start MySQL Server”按钮来启动它，启动之后，状态会变成running。

下方还有一个复选框按钮，可以设置是否在系统启动的时候自动启动MySQL，默认是勾选的，建议取消，节省开机时间。

你可以去系统偏好设置－其他－MySQL，通过这个来启动和停止MySQL服务。

### 通过终端连接MySQL
打开终端，为Path路径附加MySQL的bin目录
```
PATH="$PATH":/usr/local/mysql/bin
```

然后通过以下命令登陆MySQL（密码就是前面自动生成的临时密码，localhost：后面的是密码）
```
mysql -u root -p
```

登陆成功，但是运行命令的时候会报错，提示我们需要重设密码。
```
mysql> show databases;
```
```
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
```

修改密码，如新密码为123456
```
set PASSWORD =PASSWORD('123456');
```

再次执行 `show databases;` 就正常了。

## 安装MySQL Workbench
双击安装 mysql-workbench-community-6.3.6-osx-x86_64.dmg

点击图标进入，发现有已经创建了一个链接，这是默认创建的，密码是通过刚才的方式已经修改过的密码。输入新密码可以进入。
![MySQL Workbench](http://blogpic.at15cm.com/mysql-wb1.png)

（我是先安装好MySQL再安装好Workbench，然后再在终端里进行上文的操作）
![MySQL Workbench](http://blogpic.at15cm.com/mysql-wb2.png)

点击链接，输入密码进入
![MySQL Workbench](http://blogpic.at15cm.com/mysql-wb3.png)

来自：[Mac下MySQL与MySQLWorkbench的安装](http://www.cnblogs.com/libiyangblog/p/5186904.html)