---
title: 项目部署2：CentOS7.5基于docker安装nginx
date: 2020-05-28 20:58:15
tags: docker
categories: 服务器端
---

### 1 安装nginx

#### 1.1 查看docker已经下载的(已有的)镜像

```
docker images
##### 以下为已有的镜像，如果第一次则为空
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              9beeba249f3e        12 days ago         127MB
```



#### 1.2 去官方公有仓库查询nginx镜像

第一个也就是 STARS 数最多的就是官方镜像

```
docker search nginx
```



#### 1.3 拉取该镜像

```
docker pull nginx
```



#### 1.4 再次查看本地镜像

```
docker images
```



#### 1.5 启动该镜像，使用nginx服务，代理本机8080端口

```
docker run -d -p 8080:80 nginx
```

+ `-d`：后台运行

+ `-p`：端口映射，冒号前是本机端口，冒号后是容器端口

 访问 本机ip:8080 即可访问该页面（Welcome to nginx!），代表nginx启动成功 。



#### 1.6 关闭容器

docker stop xxxxx(容器id前几位)

```
docker stop caca8de
```





### 2 本机映射nginx文件夹

> 至此 简单的docker安装nginx并启动算是成功了，接下来就会产生一个疑问，如果我想修改nginx的配置怎么办，我想更改nginx中的资源文件怎么办？接下来给出一个最实用的办法，就是将容器中的目录和本机目录做映射，以达到修改本机目录文件就影响到容器中的文件。

#### 2.1 本机创建实例文件夹

> `/home`文件夹下新建 自定义 文件夹如folder，folder文件夹下新建nginx文件夹，nginx文件夹下新建conf.d文件夹，html文件夹，大致结构如下：



```ruby
/home
    |---folder
           |----nginx
                  |----conf.d
                  |----html
```



#### 2.2 在conf.d文件夹下新建default.conf文件，内容如下：

```
server {
    listen       80;
    server_name  localhost;
    # 原来的配置，匹配根路径
    #location / {
    #    root   /usr/share/nginx/html;
    #    index  index.html index.htm;
    #}
    # 更该配置，匹配/路径，修改index.html的名字，用于区分该配置文件替换了容器中的配置文件
    location / {
        root   /usr/share/nginx/html;
        index  index-test.html index.htm;
    }
}
```



#### 2.3 在html中编写index-test.html，用以判断文件夹映射成功，内容如下：

```html
<html>
  <body>
    <h2>it is html1</h2>
  </body>
</html>
```



#### 2.4 启动nginx(8080)，映射路径

```
docker run -d \
	-p 8080:80 \
	-v /home/folder/nginx/conf.d:/etc/nginx/conf.d \
	-v /home/folder/nginx/html:/usr/share/nginx/html \
	nginx
```

命令比较长，其实就是多加了两个参数，-v，-v的意思就是冒号前面代表本机路径，冒号后面代表容器内的路径，两个路径进行了映射，本机路径中的文件会覆盖容器内的文件。

 nginx容器内的一些文件位置：

+ 日志位置：/var/log/nginx/
+ 配置文件位置：/etc/nginx/
+ 项目位置：/usr/share/nginx/html

#### 2.5 验证

此时访问 ip:8080，发现展示的不是nginx的默认页面了，而是我们新加入的页面，这样就证明我们两个 `-v` 映射的文件夹都起作用了。





### 3 反向代理

> 此时静态页面网站已经部署上了，但是还是会显示一个端口8080出来，就十分不爽，怎么把端口干掉呢？而是换成XXXXX.com/demo1  或者 XXXXX.com/demo2这种效果呢？下面使用nginx的反向代理实现。

#### 3.1 新增文件夹conf.d2

```
/home
    |---folder
           |----nginx
                  |----conf.d
                  |----html
                  |----conf.d2
```

 我们在conf.d2中配置另一个nginx容器的配置文件，文件内容如下： 

```
server {
    listen       80;
    server_name  localhost;
    location /abc {
        # 在该位置配置反向代理，将ip/abc请求拦截，发送给8080端口，如果不是本机请使用公网ip
        proxy_pass   http://你的刚才的ip地址:8080/;
    }
}
```



#### 3.2 再启动一个nginx(80)

再启动一个nginx(80)，专门作为反向代理映射，将本机80端口代理到nginx的80端口上，并映射两端的配置文件地址。

```
docker run -d -p 80:80 -v /home/folder/nginx/conf.d2:/etc/nginx/conf.d nginx
```

 此时 访问 ip/abc 即可映射到了 ip:8080 上，成功完成反向代理。 





### 4 扩展：负载均衡

> 当有了反向代理后，自然而然就引出了`负载均衡`,下面简单实现`负载均衡`的效果，实现该效果再添加一个nginx，所以要增加一个文件夹。 

#### 4.1 新增文件夹html3

```
/home
    |---folder
           |----nginx
                  |----conf.d
                  |----html
                  |----conf.d2
                  |----html3
```

添加一个index.html文件

```html
<html>
  <body>
    <h2>it is html1</h2>
  </body>
</html>
```



#### 4.2 启动nginx(8081)

```
docker run -d -p 8081:80 -v /home/folder/nginx/conf.d:/etc/nginx/conf.d  -v /home/folder/nginx/html3:/usr/share/nginx/html nginx
```



#### 4.3 访问ip:8081

正常访问



#### 4.4 配置负载均衡

访问 ip/abc 时，平均分发到8080端口和8081端口上，即 `it is html1` 和 `it is html3` 间接出现。



配置负载均衡，那就是配置在第二次的nginx上，就是反向代理的nginx上，我们去 conf.d2 文件夹下，修改 default.conf 文件，如下：

```
upstream group1{
    server 你的刚才的ip地址:8080;
    server 你的刚才的ip地址:8081;
}

server {
    listen       80;
    server_name  localhost;
    location /abc {
        proxy_pass   http://group1/;
    }
}

```

 此时，查看所有运行中的docker容器: `docker ps` 

 然后重启该容器，`docker restart 容器id` 



#### 4.5 查看效果

 访问 ip/abc，每次刷新页面，页面都会在html1和html3中进行切换，此时负载均衡的效果就实现了。 



#### 4.6 配置负载均衡的权重

 可以使用下面的配置修改两个端口的权重(即谁被访问的概率大) 

```
upstream group1{
    server 你的刚才的ip地址:8080 weight=1;
    server 你的刚才的ip地址:8081 weight=10;
}

server {
    listen       80;
    server_name  localhost;
    location /demo1 {
        proxy_pass   http://group1/;
    }
}
```

