---
title: Mac下生成 SSH Key
date: 2020-01-15 12:57:55
tags: tools
categories: tools
---
## 第一步：检查是否已经存在SSH Key

打开电脑终端，输入以下命令：

`ls -al ~/.ssh`

会出现两种情况



## 第二步：生成/设置SSH Key

### 情况一：已经存在
终端出现文件 id_rsa.pub 或 id_dsa.pub，则表示该电脑已经存在SSH Key，此时可继续输入命令：

```
//将公钥放到剪切板
pbcopy < ~/.ssh/id_rsa.pub
```

这样你需要的 SSH Key 就已经复制到粘贴板上了.

### 情况二：不存在
终端未出现 id_rsa.pub 或 id_dsa.pub 文件，表示该电脑还没有配置SSH Key，此时需要输入命令：

`ssh-keygen -t rsa -C "your_full_name@xxxxx.com"`

（注意，这里的 your_full_name@xxxxx.com 是你自己的邮箱） 默认会在相应路径下（/your_home_path）生成id_rsa和id_rsa.pub两个文件，此时终端会显示：

```
Generating public/private rsa key pair.
Enter file in which to save the key (/your_home_path/.ssh/id_rsa):
```

连续回车即可，也可能会让你输入密码，密码就是你的开机密码

此时再输入命令：`ls -al ~/.ssh` 就会出现 id_rsa.pub 和 id_dsa.pub 两个文件


原文：[熟睡的毛毛虫](https://www.jianshu.com/p/a0c783431620)
