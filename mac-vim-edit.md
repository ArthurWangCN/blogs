---
title: mac下使用自带的vim编辑器编辑文件 
date: 2017-03-28 18:01:11
tags: 工具
categories: 工具
---
Macbook Pro自带vim编辑器，下面就介绍一下如何使用 MBP 自带vim编辑器的使用方法。

1. 检测vim
打开终端，输入 `vim`，显示出 vim 的简介：
![vim简介](http://blogpic.at15cm.com/vim-1.jpeg)

2. 使用vim
vim有3种模式：
+ 普通模式
该模式下面可以输入命令，基本的控制文件的命令使用:开头的
保存文件 `:w`，即为保存
退出编辑模式 `:q`

+ 编辑模式
输入i，进入编辑模式

+ 可视模式
输入v进入可视模式：
该模式下可以复制粘贴，在编辑模式以及可视模式下按下esc键回到普通模式

**退出vim**
首先输入 `:q`，然后按 `enter` 

**进入文件夹**
用cd命令到达你想要进行编辑文件的文件夹
```
cd blog
```

**创建文件夹**
```
mkdir myvim
```

**新建一个vim文件并进入**
```
vim hello.cpp
```