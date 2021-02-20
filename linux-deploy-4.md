---
title: 项目部署4：最终篇
date: 2020-05-29 01:47:53
tags: docker
categories: 服务器端
---



### 1 打包后的文件放到nginx中

#### 1.1 停掉原先启动的端口

先把原来启动的8080端口的docker容器停止掉：

```
docker stop xxxx
```



#### 1.2 重新运行

> 注意！此时你一定要在项目的根目录中哦，不然的话也可以根据你目前的目录简单调整启动命令。 

```
docker run -d -p 8080:80 -v $PWD/dist:/usr/share/nginx/html nginx
```

这里：`$PWD`是指的当前路径，上面的警告的意思是启动命令已经规定了是当前项目根目录下的`dist`文件夹(`$PWD/dist`)，如果不在项目根目录，会出现一些问题。 



#### 1.3 检查是否成功

```
docker ps
```

 可以看到 正在运行 这时候访问浏览器测试8080端口是否成功部署静态页面。 





### 2 通过脚本简化流程

#### 2.1 增加前端启动命令

为什么要加上前端的启动命令呢？
我们分析一下：前端代码改动后我们会进行下面几步操作：

1. git pull  拉取最新代码
2. yarn build或者npm run build，进行打包
3. 停止原来的nginx容器，启动新的nginx容器（除了第一次启动时外，非必须）

```
git pull

yarn --registry=https://registry.npm.taobao.org/ && yarn build

#删除容器
docker rm -f demo1 &> /dev/null

#启动容器
docker run -d --restart=on-failure:5\
    -p 8080:80 \
    -v $PWD/dist:/usr/share/nginx/html \
    --name demo1 nginx
```

 这样的话 我们可以将这些操作合并在一个sh文件中，以后可能会有更多的命令，都可以放在一起 



#### 2.2 根目录新增start.sh

将上面命令存到根目录 start.sh 文件中



#### 2.3 获取最新代码并执行脚本

提交代码到git后，服务器git pull拉下来最新代码。

执行启动脚本：

```
$ sh start.sh
```

 这里简单实用了linux中的sh脚本代替我们频繁重启中额外的操作 。





### 3 nginx反向代理丢失js、css问题

我们已经可以通过 `http://118.25.194.49:8080` 访问到我们的项目，此时，假设我们要上线，8080端口暴露着实不好看，我们希望是类似`http://118.25.194.49/demo1/`这样的格式进行访问项目，看着比较舒服，这样怎么做呢？

#### 3.1 反向代理

```
server {
    listen       80;
    server_name  localhost;
    location /demo1 {
        proxy_pass   http://118.25.194.49:8080/;
    }
}
```

 看似这种方法直接就解决了这个痛点问题，这个配置文件的含义就是将/demo1的请求转发到8080端口上，十分完美，我们重启nginx会发现js和css文件都找不到了。

我们发现，项目中的静态文件为啥都直接去根目录去找呢？问题出在打包的配置文件上：
我们本地执行打包命令`yarn build`，然后去根目录的dist文件夹中查看index.html文件， 发现所有的外部文件都是去根目录开始的，这就会产生上面的问题。 



#### 3.2 解决文件找不到

 解决问题肯定是去整理刚才的路径，我们这里修改配置文件中的`发布路径` ：

+ 老版本vue：修改config文件夹下的index.js

```javascript
build: {
	assetsPublicPath: './'
}
```

+ 新版本vue： 需要新增vue.config.js文件并设置

```
module.exports = {
	publicPath: './'
}
```

注意：这里 `./` 是假设不是在子路径，如果是子路径如 `http://ip/xxx` 需要把publicPath改为 `./xxx/`。

 这时候，再次打包，看一下dist里面的index.html文件，发现已有了效果。  此时，提交代码到github，服务器拉下来代码，运行，查看效果，成功。 

