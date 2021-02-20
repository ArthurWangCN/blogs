---
title: 项目部署3：CentOS安装git和node
date: 2020-05-29 00:52:20
tags: docker
categories: 服务器端
---

### 1 安装Git并拉取代码

> 首先我们先保证有一个项目上传到如github或gitee等项目托管平台 

#### 1.1 登录服务器，并执行安装git的命令

```
yum install -y git
```



#### 1.2 检查是否安装成功 

#### 等待一下，安装结束后，检查是否安装成功，出现版本号即代表安装成功

```
git version
```



#### 1.3 git配置

+ 配置一个用于提交代码的用户，输入命令： 

```
git config --global user.name "Your Name"
```

+ 配置一个用户邮箱，输入命令： 

```
git config --global user.email "email@example.com"
```

+ 生成公钥和私钥，输入命令后一路回车即可： 

```
ssh-keygen -t rsa -C "youremail@example.com"
```

![ssh-kegen](https://upload-images.jianshu.io/upload_images/6555843-b83df6da4fc06f6c.png?imageMogr2/auto-orient/strip|imageView2/2/w/771/format/webp)

这个文件稍后会用到



#### 1.4 添加SSH key

1. 打开github，找到setting；
2. 添加SSH key —— SSH adn GPG keys，title字段自定义，key字段填 /root/.ssh/id_rsa.pub中的内容；



#### 1.5 拉取代码

在服务器新建目录，拉取项目代码,使用 **git clone** 命令

```
git clone git@gitee.com:awstudio/travel.git
```

至此，服务器端项目代码拉取完毕，接下来在本地编译该代码，生成静态文件





### 2 安装node并编译项目

#### 2.1  安装node

下载nodejs最新的tar包 ， 可以在下载页面[https://nodejs.org/en/download/](https://links.jianshu.com/go?to=https%3A%2F%2Fnodejs.org%2Fen%2Fdownload%2F)中找到下载地址。然后执行指令 ：

```
$ wget https://nodejs.org/dist/v9.3.0/node-v9.3.0-linux-x64.tar.xz
```

解压缩：

```
$ tar -xvf node-v9.3.0-linux-x64.tar.xz
```



#### 2.2  将目录软链接到全局环境下

 先确认你nodejs的路径，例如路径为 `~/node-v9.3.0-linux-x64/bin`。确认后依次执行 

```
$ ln -s ~/node-v9.3.0-linux-x64/bin/node /usr/bin/node
$ ln -s ~/node-v9.3.0-linux-x64/bin/npm /usr/bin/npm
```

命令后面的 `/usr/bin/node` 是固定的， 注意ln指令用于创建关联（类似与Windows的快捷方式）必须给全路径，否则可能关联错误。



#### 2.3 测试是否安装成功 

```
node -v
```

如果打印出版本号就成功了，npm同理



#### 2.4 安装yarn

```
$ npm install yarn -g
$ ln -s ~/node/node-v9.3.0-linux-x64/bin/yarn /usr/bin/yarn
```



#### 2.5 打包前端项目

进入到前端项目目录中，安装依赖：

```
npm install
或者
yarn
```

打包：

```
npm run build
或者
yarn build
```



#### 2.6 检查打包后的静态文件

打包后多了个dist文件夹，里面就是打包好的静态文件

```
[root@VM_0_16_centos dist]#cd dist
[root@VM_0_16_centos dist]# ls
favicon.ico  index.html  static
```

