---
title: 怎么在Hexo博客安装theme
date: 2017-02-18 21:59:54
tags: [hexo, 博客搭建]
categories: 博客搭建
---
## 安装theme
你可以到Hexo官网主题页去搜寻自己喜欢的 `theme`。这里以 `hexo-theme-next` 为例。  
终端cd到 `blog` 目录下执行如下命令：
  
```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

将 `blog` 目录下 `_config.yml` 里 `theme` 的名称`landscape` 修改为 `next`  

终端cd到 `blog` 目录下执行如下命令(每次部署文章的步骤)：  
  
1. 清除缓存文件 (db.json) 和已生成的静态文件 (public) `$ hexo clean`  
2. 生成缓存和静态文件 `$ hexo g`  
3. 重新部署到服务器 `$ hexo d`  


## next主题配置  
### 主题风格设定  
通过修改next主题下的 `_config.yml` 的 `scheme` 字段，配置不同的风格。默认使用的是 `Muse`，本站使用的是 `Pisces`。  

### 菜单设置  
通过修改next主题下的 `_config.yml` 的 `menu` 字段，选定显示的菜单项。其中，`home` 代表主页，`categories` 代表分类页，`about` 代表关于页面，`archives` 代表归档页，`commonweal` 代表404页面（page not found时候显示的页面）。
  
菜单项文本修改是在对 `next` 主题下的 `language` 文件夹下的文件进行修改，若当前语言是简体中文，直接修改 `language/zh-Hans.yml` 里的对应字段即可。  

### 头像设置  
在主题下的 `source/images/` 下放置头像文件 `avatar.gif` 即可。  

### 设置文章代码主题  
Next主题总共支持5种主题，默认主题是白色的 `normal`。通过修改next主题下的 `_config.yml` 的 `highlight` 字段，来设置代码主题。  

### 添加标签页面  
前面通过修改next主题下的 `_config.yml` 文件中的 `menu` 选项，可以在主页面的菜单栏添加标签选项，但是此时点击标签，跳转的页面会显示page not found。  

添加标签页面的具体方法是：  
   
1.新建页面  
终端cd到 `blog` 目录下执行如下命令：  

```
$ hexo new page tags
```

输入命令后，在 `blog/source` 下会新生成一个新的文件夹 `tags`，在该文件夹下会有一个 `index.md` 文件。  

2.设置页面类型  
在上步新生成的 `blog/source/tags/index.md` 中添加 `type: "tags"`，index.md文件内容如下：
  
```
---
title: tags
date: 2017-01-01 00:00:00
type: "tags"
---
```

3.设置具体文章的tags  
当要为某一篇文章添加标签，只需在 `blog/source/_post` 目录下的具体文章的tags中添加标签即可，如：  

```
---
title: Mac上搭建基于Github的Hexo博客
date: 2017-01-01
tags: [Github, Hexo]
---
```

### 添加分类页面  
步骤与 `添加标签页面` 类似，把 `tags` 改成 `categories` 即可，这里不再赘述。

### 添加关于我页面  
步骤与 `添加标签页面` 类似，把 `tags` 改成 `about` 即可，这里不再赘述。`关于我` 的页面是你的个人介绍，自己发挥无限的想象吧。

我的 [关于我](https://legendaryarthur.github.io/about/)


##引入第三方服务
### 加入评论功能
本站点使用的是 [多说](http://duoshuo.com/)。加入评论功能的步骤如下：

+ 登录 [多说](http://duoshuo.com/)，填写表单，创建站点

![创建多说站点](https://syd192.github.io/photo/20161115/4.png)

图片中红框圈中的框中内容就是下一步 `duoshuo_shortname` 字段的值

+ 添加 `duoshuo_shortname`

在站点的 `blog/_config.yml` 中加入 `duoshuo_shortname` 字段，值为第一步红框里的内容。

复制创建站点后跳转网页的代码，并粘帖到您网页代码 `<body>` 与 `</body>` 间的任意位置。如果您的网站使用模板，请粘帖到模板代码中。

加入评论后效果如下：

![加入评论](https://syd192.github.io/photo/20161115/5.png)

### 加入分享功能
 本站点使用的是多说。加入分享功能的步骤如下：

在站点的 `blog/_config.yml` 中加入 `duoshuo_share` 字段，值为true。

加入分享后效果如下：

![加入分享功能](https://syd192.github.io/photo/20161115/6.png)

### 加入站点内容搜索功能
本站点使用的是 `Local Search`。加入站点内容搜索功能步骤如下：

+ 安装 `hexo-generator-searchdb`

```
$ npm install hexo-generator-searchdb --save
```

注意：安装时应在站点根目录下，即 `blog` 目录下

+ 添加 `search` 字段

在站点 `blog/_config.yml` 中添加 `search` 字段，如下：

```
search: 
	path: search.xml
	field: post
	format: html
	limit: 10000
```

效果如下：
![搜索功能](https://syd192.github.io/photo/20161115/7.png)

### 加入数据统计与分析功能

本站点使用的是 [百度统计](http://tongji.baidu.com/web/welcome/login)。加入数据统计与分析功能步骤如下：

+ 注册站长账号并登陆

在这里注册站长账号，并填写信息，网站域名和网站首页以下图为例来填写，注册完成后并登陆。

![百度统计](https://syd192.github.io/photo/20161115/8.png)

+ 在跳转的页面中会显示下图，复制hm.js后的id值

![](https://syd192.github.io/photo/20161115/9.png)


+ 添加 `baidu_analytics` 字段

在站点 `blog/_config.yml` 中添加 `baidu_analytics` 字段，值为上步复制的id值。

至此，该功能已成功加入，大约过20min后在百度统计上可以看到站点的访问情况，如下图：

![百度统计](https://syd192.github.io/photo/20161115/10.png)


## 更新
现在 `多说` 宣布关闭，建议移除掉。


## 参考连接
+ [hexo-theme-next](https://github.com/iissnan/hexo-theme-next/)
+ [nexT使用文档](http://theme-next.iissnan.com/)
+ [Hexo的Next主题配置](http://blog.csdn.net/zuoziji416/article/details/53204478)
