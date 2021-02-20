---
title: 配置Android的adb环境变量
date: 2018-06-04 10:48:44
tags: [游戏开发, 工具]
categories: 工具
---
>近期小游戏上线要求做一下真机性能测试，用到了 `H5Arugs` ，要求在电脑上配置好adb环境变量，这里整理了一下配置方法。

### 打开环境变量配置窗口
右击计算机，属性-高级系统设置-环境变量。

### 添加android系统环境变量
在系统变量下点击新建按钮，输入环境变量名android（自己的习惯命名），将android开发工具的路径导入（或者直接添加到Path中），如图：

![adb环境变量](http://blogpic.at15cm.com/adb-env-settings.png)

### 在Path中添加刚刚添加的环境
选择系统变量中Path，点击编辑按钮，输入刚刚建好的环境，方法和配置java一样，记住要加两个百分号（同样的如果直接把路径添加到Path中的话此步骤省略），如图：

![adb环境变量](http://blogpic.at15cm.com/adb-system-var.jpg)

### 测试环境变量
首先打开运行命令，运行在开始菜单中就有，如果找不到可以在开始中搜索即可，也可以直接按住win+R快捷键，打开运行。在运行中输入cmd，调用命令操作窗口。进入后输入adb查看运行结果，如图所示就是配置成功了：

![adb环境变量](http://blogpic.at15cm.com/adb-cmd-check.jpg)

