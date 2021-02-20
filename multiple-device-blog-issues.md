---
title: 多设备下同步博客问题汇总
date: 2018-05-31 18:35:43
tags: [Github, 博客搭建, Hexo]
categories: 工具
---

## Deprecation Warning
**DeprecationWarning: fs.SyncWriteStream is deprecated**

**分析：** 
node和hexo插件的版本带来的问题：在node8.x的版本中，fs.SyncWriteStream被弃用了。

**解决：**
更新如下插件：

```
npm install hexo-fs --save
npm install hexo-deployer-git@0.3.1 --save
npm install hexo-renderer-ejs@0.3.1 --save
npm install hexo-server@0.2.2 --save```


## MarkdownPad 2 在win10下出错：HTML 渲染错误
这个问题一般多见于win8（*当然现在win10也有，官方文档该更新啦）。错误的表现形式即：不能实时预览Markdown生成的HTML页面。

**解决：**
为了修复这个问题，你需要安装这么一个SDK工具包，[点击链接获取](markdownpad.com/download/awesomium_v1.6.6_sdk_win.exe)。

资源：[Markdown破解版](https://jingyan.baidu.com/article/ea24bc39b985dfda63b33176.html)


## hexo d部署博客报错taskcanceledexception
使用hexo d命令部署博客的时候发生了下面的异常：
![taskcanceledexception](http://awstudio.cn/www/blogPic/taskcanceledexception.jpg)
**解决：**
[解决hexo d命令提交部署博客发生的TaskCanceledException异常](https://www.jianshu.com/p/cc38fc9493d4)

我又走了一遍 `hexo clean -> hexo g -> hexo d` 结果就解决了，真是妖怪多！



## Git相关
### 合并冲突
CONFLICT (content): Merge conflict in ***
Automatic merge failed;

[解决冲突](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000)

扩展：[多人协作](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013760174128707b935b0be6fc4fc6ace66c4f15618f8d000)


### git pull 报错 No remote repository specified
其实出问题的原因是.git/config的配置出问题了。

**解决：**

修改“.git”文件夹里面的“config”文件的url就可以了。[详情](https://blog.csdn.net/myhuashengmi/article/details/52208032)


### git push时报错filename too long
**解决：**

命令行输入：
`git config core.longpaths true`

之后再进行 git 的 **push** 命令


### warning: LF will be replaced by CRLF
**分析：**
[git如何避免”warning: LF will be replaced by CRLF“提示？](https://www.zhihu.com/question/50862500/answer/123197258)


**解决：**

设置 core.autocrlf=true, 只要保持工作区都是纯 CRLF 文件，编辑器用 CRLF 换行，
就不会出现警告了


## ERROR Local hexo not found in xxx
Hexo搭建博客之后用git已经将所有的source都同步到了git上，在另一台电脑上将源代码clone下来之后，直接执行 hexo server,出现错误

```
ERROR Local hexo not found in E:\blog
ERROR Try running: 'npm install hexo --save'```

**解决：**

```
cd E:\blog
npm install
hexo server```

**分析：**
`.gitignore` 文件里面忽略了 `node_modules` 文件夹，所以这个文件夹没有更新上去。所以用npm重新安装即可。


## hexo本地测试运行重启后页面空白,WARN No layout
把下载的主题文件夹移到"themes"文件夹下，然后在配置文件 `_config.yml` 文件中将文件夹名作为theme的配置

[知乎](https://www.zhihu.com/question/38781463?sort=created)


## 【Hexo异常】fatal: in unpopulated submodule '.deploy_git'
先安装下相关的依赖：

`npm install hexo-deployer-git –save`

如果不行就删掉，然后重新生成和部署。

```
rm -rf .deploy_git
hexo g
hexo d```


## Hexo实现多终端同步管理
参考链接：

+ [Hexo实现多终端同步管理](https://blog.csdn.net/dxxzst/article/details/76135750)
+ [多台电脑操作hexo个人网站](https://blog.csdn.net/Zhangxiaorui_9/article/details/79723288)
+ [利用Hexo在多台电脑上提交和更新github pages博客](https://www.jianshu.com/p/0b1fccce74e0)
+ [换了电脑如何使用hexo继续写博客](https://blog.csdn.net/lvonve/article/details/79587321)

## 总结
总之坑比较多，主要是自己对Hexo，Git，Github等等原理理解不到位，接下来多注重原理吧。

