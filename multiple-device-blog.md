---
title: 如何实现在多台电脑同步写Hexo博客
date: 2018-06-22 14:13:26
tags: [Github, 博客搭建, Hexo]
categories: 工具
---
>最开始接触 github + hexo 搭建博客用的是 MBP，自从转到游戏开发后在单位接触越来越多的新知识，又迫切需要在工作中记录些东西，最简单的方法就是在单位电脑上写博客，就折腾了一下。

## 思路
主要的思路是 **利用git分支实现**。

hexo生成的静态博客文件默认放在master分支上。hexo的源文件（部署环境文件）可以都放在hexo分支上（可以新创建一个hexo分支），换新电脑时，直接 git clone hexo 分支即可。


## hexo搭建博客原理
hexo博客的部署环境目录：

![部署环境目录](http://blogpic.at15cm.com/mdb-1.png)

hexo博客的目录结构解析：

![目录结构解析](http://blogpic.at15cm.com/mdb-2.png)

上传到github上的文件：

![github目录](http://blogpic.at15cm.com/mdb-3.png)

1. hexo 帮助把博客发送到 github，同时把md文件转换成网页文件。
2. hexo 目录下的文件和 github 上的文件是不同的，public 文件夹的文件通过 `hexo d` 上传到 github 去了，其他的文件则留在本地目录下。



## 初始搭建博客电脑上的操作
### 准备工作
首先确保自己已经使用hexo在github搭建好了自己的个人博客，github仓库中如下图显示：

![github目录](http://blogpic.at15cm.com/mdb-4.png)

### 新建分支
**对 username.github.io 仓库新建 hexo 分支，并克隆**

在 Github 的 username.github.io 仓库上新建一个 xxx 分支，并切换到该分支，并在 `该仓库->Settings->Branches->Default branch` 中将默认分支设为 xxx，save保存；然后将该仓库克隆到本地，进入该 username.github.io 文件目录。

![新建分支](http://blogpic.at15cm.com/mdb-5.png)

<br />

![新建分支](http://blogpic.at15cm.com/mdb-6.png)

完成上面步骤后，在当前目录使用 Git Bash 执行 `git branch` 命令查看当前所在分支，应为新建的分支 xxx：

```
$ git branch
*hexo```

### 本地博客的部署文件拷贝
先将本地博客的部署文件（Hexo目录下的全部文件）全部拷贝进 username.github.io 文件目录中去。

接下来，进入 username.github.io 文件目录下，将该目录下的全部文件提交到 xxx 分支，提交之前需注意：

将themes目录以内中的主题的.git目录删除（如果有），因为一个git仓库中不能包含另一个git仓库，提交主题文件夹会失败。

可能有人会问，删除了themes目录中的.git不就不能 git pull 更新主题了吗，很简单，需要更新主题时在另一个地方 git clone 下来该主题的最新版本，然后将内容拷到当前主题目录即可。

### 提交hexo分支
执行 `git add .` 、 `git commit -m 'back up hexo files'`（引号内容可改）、 `git push` 即可将博客的 hexo 部署环境提交到 GitHub 个人仓库的 xxx 分支。

现在可以在GitHub上的 username.github.io 仓库看到两个分支的差异了。

![提交hexo分支](http://blogpic.at15cm.com/mdb-7.png)

<br />

![提交hexo分支](http://blogpic.at15cm.com/mdb-8.png)

master 分支和 xxx 分支各自保存着一个版本，master 分支用于保存博客静态资源，提供博客页面供人访问；xxx 分支用于备份博客部署文件，供自己维护更新，两者在一个GitHub仓库内互不冲突。


## 新电脑上的操作
至此，你的博客已经可以在其他电脑上进行同步的维护和更新了，方法很简单：

1. 将新电脑的生成的 ssh key 添加到 GitHub 账户上
2. 在新电脑上克隆 username.github.io 仓库的 xxx 分支到本地，此时本地 git 仓库处于 xxx 分支
3. 切换到 username.github.io 目录，执行 npm install (由于仓库有一个 .gitignore 文件，里面默认是忽略掉 node_modules 文件夹的，也就是说仓库的 hexo 分支并没有存储该目录[也不需要]，所以需要install下)

![新电脑上的操作](http://blogpic.at15cm.com/mdb-9.png)

到这里了就可以开始在自己的电脑上写博客了。

+ 编辑、撰写文章或其他博客更新改动
+ 依次执行git add .、git commit -m 'back up hexo files'（引号内容可改）、git push指令，保证xxx分支版本最新
+ 执行hexo d -g指令（在此之前，有时可能需要执行hexo clean），完成后就会发现，最新改动已经更新到master分支了，两个分支互不干扰！


## 初始电脑上更新并提交博客
**注意：每次换电脑进行博客更新时，不管上次在其他电脑有没有更新，最好先git pull**

![初始电脑上](http://blogpic.at15cm.com/mdb-10.png)

按照之前的方法写自己博客，然后将目录切换下username.github.io下，此时需要安装一下npm install，最后执行hexo g、hexo s、hexo d等命令即可提交成功。

## 说明
本片文章来自[网络](https://www.jianshu.com/p/0b1fccce74e0)，图片也来源于原网站，只做学习，别无他用。


