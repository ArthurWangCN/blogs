---
title: Mac上搭建基于GitHub的Hexo博客
tags: [Github, hexo, 博客搭建]
categories: 博客搭建  
---
终于成功在github上搭建了属于自己的博客，从进入前端这个行业就开始记笔记，刚开始是用笔记本，随着学习的进展也开始使用印象笔记和新浪微博记录学习的心路历程。去网上搜了一下个人博客用什么比较好，当然很多人说github+hexo搭建博客是最完美的，自己的博客，没有广告，很干净，于是便开始搭建博客之路，这里记录一下搭建的过程，主要参考了 [与佳期的个人博客](http://gonghonglou.com/2016/02/03/firstblog) 搭建的过程。

## 环境配置
[Hexo](https://hexo.io/docs) 官网上有对Hexo安装及使用的详细介绍,这里来讲述自己安装的亲身体验。

### 1.Node.js
用来生成静态页面。移步 [Node.js官网](https://nodejs.org/en/)，下载 `Rocommended For Most Users`(推荐给大多数用户)，按照步骤安装即可。

### 2.Git
用来将本地 `Hexo` 内容提交到 `Github` 上。`Xcode` 自带 Git，这里不再赘述。如果没有 Xcode 可以参考 Hexo 官网上的安装方法。

## 安装Hexo
当 `Node.js` 和 `Git`都安装好后就可以正式安装Hexo了，终端执行如下命令：  

```
$ sudo npm install -g hexo
```

输入管理员密码（Mac登录密码）即开始安装 (`sudo`:linux系统管理指令 `-g`:全局安装)  
  
>注意：Hexo官网上的安装命令是 `$ npm install -g hexo-cli` ，安装时不要忘记前面加上 `sudo`，否则会因为权限问题报错。  

## 初始化  
终端 `cd` 到一个你选定的目录，执行`hexo init`命令：  
  
```
$ hexo init blog
```

`blog` 是你建立的文件夹名称。cd到 `blog` 文件夹下，执行如下命令，安装npm：  

```
$ npm install
```

执行如下命令，开启hexo服务器：  

```
$ hexo s
```

此时，浏览器中打开网址 <http://localhost:4000>，能看到如下页面：

![hexo](http://image.gonghonglou.com/firstblog/hexo4000.png)
  
本地设置好后，接下来开始关联 `Github`。  


## 关联Github
### 1.创建仓库  
登录你的Github帐号（如果没有帐号就去 [Github官网](https://github.com) 注册一个），然后 [新建仓库](http://jingyan.baidu.com/article/c843ea0ba1110d77921e4a7e.html) ，名为 `用户名.github.io` 固定写法，如 `legendaryarthur.github.io`。

本地的`blog`文件夹下内容为：  
  
```
_config.yml
db.json
node_modules
package.json
scaffolds
source
themes
```

终端cd到`blog`文件夹下，用编辑器打开 `_config.yml`，命令如下：
`$ vim _config.yml`

打开后往下滑到最后，修改成下边的样子：  

```
deploy:
	type: git
	repository: https://github.com/legendaryarthur/legendaryarthur.github.io.git
	branch: master
```
  
你需要将 `repository` 后的 `legendaryarthur` 换成你自己的用户名。hexo 3.1.1 版本后 type 值为 git。  

>注意：在配置所有的`_config.yml`文件时（包括theme中的），在所有的冒号`:`后边都要加一个空格，否则执行hexo命令会报错，切记！！！  

在`blog`文件夹目录下执行生成静态页面命令：  

```
$ hexo generate  或者：hexo g
```

```
此时若出现如下报错：
ERROR Local hexo not found in ~/blog
ERROR Try runing: 'npm install hexo --save'
则执行命令：
npm install hexo --save
若无报错，自行忽略此步骤。
```


在执行配置命令：

```
$ hexo deploy   或者：hexo d
```

若执行命令 `hexo deploy` 仍然报错：无法连接git或找不到git，则执行如下命令来安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)：

```
$ npm install hexo-deployer-git --save
```

再次执行 `hexo generate` 和 `hexo deploy` 命令。  

若你未关联 `Github`，则执行`hexo deploy`命令时终端会提示你输入 Github 的用户名和密码，即

```
Username for 'https://github.com':
Password for 'https://github.com':
```

`hexo deploy` 命令执行成功后，浏览器中打开网址[http://legendaryarthur.github.io](http://legendaryarthur.github.io)（将`legendaryarthur`换成你的用户名）能看到和打开 `http://localhost:4000` 时一样的页面。  

为避免每次输入Github用户名和密码的麻烦，可参照第二节方法  

## 添加ssh key到Github  
### 检查SSH keys是否存在Github  
  
执行如下命令，检查SSH keys是否存在。如果有文件`id_rsa.pub`或`id_dsa.pub`，则直接进入步骤1.3将 `SSH key` 添加到 `Github` 中，否则进入下一步生成 `SSH key`。  

```
$ ls -al ~/.ssh
```

### 生成新的ssh key  
执行如下命令生成public/private rsa key pair，注意将`your_email@example.com`换成你自己注册Github的邮箱地址。  

```
$ ssh-keygen -t rsa -C "your_email@example.com"
```

默认会在相应路径下（`~/.ssh/id_rsa.pub`）生成 `id_rsa` 和 `id_rsa.pub` 两个文件。   

### 将ssh key添加到Github中   

Find前往文件夹 `~/.ssh/id_rsa.pub` 打开 `id_rsa.pub` 文件，里面的信息即为 `SSH key`，将这些信息复制到 `Github` 的 `Add SSH key` 页面即可。

进入 `Github` –> `Settings` –> `SSH keys` –> `add SSH key`:

Title 里任意添一个标题，将复制的内容粘贴到Key里，点击下方 `Add key` 绿色按钮即可。  

## 发布文章  
终端cd到 `blog` 文件夹下，执行如下命令新建文章：  

```
$ hexo new "postName"
```
 
名为 `postName.md` 的文件会建在目录 `/blog/source/_posts` 下，`postName` 是文件名，为方便链接不建议掺杂汉字。你当然可以用vim来编辑文章，我用的是Mou编辑器，支持预览。  

文章编辑完成后，终端cd到 `blog` 文件夹下，执行如下命令来发布：  

```
hexo generate  //生成静态页面 
hexo deploy    //将文章部署到Github
```

至此，Mac上搭建基于 Github 的 Hexo 博客就完成了。下面的内容是介绍安装theme和绑定个人域名，如果有兴趣且还有耐心的话，请继续吧。  

  
 
## 参考文献
1. [与佳期的个人博客](http://gonghonglou.com/2016/02/03/firstblog/)  
2. [奔跑的牛](https://niuxiaokui.github.io/)
