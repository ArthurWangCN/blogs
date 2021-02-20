---
title: Flash Builder 4.6 提示 Failed to create the Java Virtual Machine 解决办法
date: 2018-06-19 14:24:48
tags: [工具, 游戏开发]
categories: 工具
---

整理了一下跟随了我五年的电脑，好多的回忆，也好多的数字垃圾，清了好多东西，瞬间感觉清爽了。晚上打开 Flash Builder 想撸代码，结果刚打开就弹出 “Failed to create the Java Virtual Machine”，真是妖怪咯~嗯，创建java虚拟机失败，去网上搜一下，方法如下：

 

将Flash Builder 4.6 安装目录下面的下面两个文件 ：`FlashBuilder.ini` 和 `FlashBuilderc.ini` 里面的内容改一下：

FlashBuilder.ini改为：

```
-nl
zh_CN
-startup
eclipse/plugins/org.eclipse.equinox.launcher_1.2.0.v20110502.jar
--launcher.library
eclipse/plugins/org.eclipse.equinox.launcher.win32.win32.x86_1.1.100.v20110502
--launcher.defaultAction
openFile
-vmargs
-Xms256m
-Xmx512m
-XX:MaxPermSize=128m
-XX:PermSize=64m
-Dorg.eclipse.equinox.p2.reconciler.dropins.directory=eclipse/dropins
-Declipse.application=com.adobe.flexbuilder.standalone.FlashBuilderApplication```


FlashBuilderC.ini改为：

```
-startup
eclipse/plugins/org.eclipse.equinox.launcher_1.2.0.v20110502.jar
--launcher.library
eclipse/plugins/org.eclipse.equinox.launcher.win32.win32.x86_1.1.100.v20110502
--launcher.defaultAction
openFile
-nl
en_US
-vmargs
-Xms128m
-Xmx128m
-XX:MaxPermSize=128m
-XX:PermSize=64m
-Dorg.eclipse.equinox.p2.reconciler.dropins.directory=eclipse/dropins
-Declipse.application=com.adobe.flexbuilder.standalone.FlashBuilderApplication```
 

Flash Builder 4.6 安装目录下面的eclipse文件夹下面的 eclipse.ini文件

改为：

```
-clean
-startup
plugins/org.eclipse.equinox.launcher_1.2.0.v20110502.jar
--launcher.library
plugins/org.eclipse.equinox.launcher.win32.win32.x86_1.1.100.v20110502
-nl
en_US
-vmargs
-Xms128m
-Xmx128m
-XX:MaxPermSize=128m
-XX:PermSize=64m
-Djava.net.preferIPv4Stack=true
-Dorg.eclipse.equinox.p2.reconciler.dropins.directory=dropins
-Declipse.application=com.adobe.flexbuilder.standalone.FlashBuilderApplication
-Dfile.encoding=UTF-8```
 

总结一下就是把三个文件中 `MaxPermSize` 的值改为 `128m`， `-XX:MaxPermSize=128M` 表示 JVM最大允许分配的非堆内存，按需分配

 

参考链接：

+ [Flash Builder 4.6 提示 Failed to create the Java Virtual Machine 解决办法](https://blog.csdn.net/cyyingsun/article/details/7901073)
+ [Xms Xmx PermSize MaxPermSize 区别](https://blog.csdn.net/luxun2014/article/details/42110725)