---
title: hexo博客--绑定个人域名
date: 2017-02-20 23:28:17
tags: [hexo, 博客搭建]
categories: 博客搭建
---
现在使用的域名是Github提供的二级域名，也可以绑定为自己的个性域名。购买域名，可以到[GoDaddy官网](https://sg.godaddy.com/zh/)，网友亲切称呼为：狗爹，也可以到[阿里万网](https://wanwang.aliyun.com/)购买。我是在万网买的，可直接在其网站做域名解析。

## Github端
在 `/blog/themes/next/source` 目录下新建文件名为：`CNAME` 文件，注意没有后缀名！直接将自己的域名如：`legendaryarthur.cn` 写入。

终端cd到blog目录下执行如下命令重新部署。

>注意：网上许多都是说在Github上直接新建CNAME文件，如果这样的话，在你下一次执行hexo d部署命令后CNAME文件就消失了，因为本地没有此文件。

## 域名解析
如果将域名指向一个域名，实现与被指向域名相同的访问效果，需要增加CNAME记录。登录万网，在你购买的域名后边点击：`解析 –> 添加解析`

记录类型：`CNAME`

主机记录：将域名解析为 `example.com`（不带www），填写@或者不填写

记录值：`legendaryarthur.github.io.` (不要忘记最后的 `.`，`legendaryarthur` 改为你自己的用户名)，点击保存即可，如下图：

![域名解析](http://blogpic.at15cm.com/alidomain.png)

此时，点击访问 <http://legendaryarthur.cn> 和访问 <http://legendaryarthur.github.io> 效果一致。

完美收工！

