---
title: 项目部署1：CentOS7.5 安装docker
date: 2020-05-28 18:58:15
tags: docker
categories: 服务器端
---

Docker 支持以下的 64 位 CentOS 版本：

- CentOS 7
- CentOS 8
- 更高版本...

### 1. 卸载旧的版本

> 较旧的 Docker 版本称为 docker 或 docker-engine 。如果已安装这些程序，请卸载它们以及相关的依赖项。 

```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 2. 安装 Docker Engine-Community

> 您可以根据需要以不同方式安装Docker CE：
> 1、大多数用户 设置Docker的存储库并从中进行安装，以便于安装和升级任务。**这是推荐的方法。**
> 2、有些用户下载RPM软件包并 手动安装并完全手动管理升级。在没有互联网的情况下，安装Docker的情况下非常有用。
> 3、在测试和开发环境中，一些用户选择使用自动 便捷脚本来安装Docker。 

#### 2.1 通过存储库安装

在新主机上首次安装 Docker Engine-Community 之前，需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker。 

1、设置存储库，安装所需的包

```
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

2、设置稳定的存储库

```
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

3、安装DOCKER CE
(1)列出所有的docker ce,选择制定的版本

```
yum list docker-ce --showduplicates | sort -r
```

(2)选择制定版本的,使用（-）链接

```
yum install docker-ce-<VERSION STRING>`
yum install docker-ce-18.06.1.ce-3.el7  
```

(3)直接yum —— 默认安装最新版本

```
yum -y install docker-ce 
```

4、启动docker容器

```
systemctl start docker
docker version #查看版本
```

5、测试docker 容器是否成功

```
docker run hello-world
```

打印出 ==> Hello from Docker! 
即安装成功

#### 2.2 通过rpm包安装

1、如果您无法使用Docker的存储库来安装Docker，则可以下载.rpm适用于您的发行版的 文件并手动安装。每次要升级Docker时都需要下载新文件。
2、转到`https://download.docker.com/linux/centos/7/x86_64/stable/Packages/`并下载.rpm要安装的Docker版本的文件。
3、下载到服务器本地，`yum -y install /path/package.rpm`
4、启用 `docker shell>systemctl start docker`
5、运行 `docker shell> docker run hello-world`

#### 2.3 通过脚本安装docker

1、脚本的下载
Docker在get.docker.com 和test.docker.com上提供了便捷脚本，用于快速，非交互地将Docker CE的边缘和测试版本安装到开发环境中
`github: https://github.com/docker/docker-install` 不建议生产环境使用

注意：
1、脚本需要root用户才能运行
2、脚本自动检测系统版本，不需要修改参数
3、不提动指定版本，会安装最新版本



### 3. 卸载 Docker CE

1、卸载Docker包

```
yum -y remove docker-ce
```



2、主机上的图像，容器，卷或自定义配置文件不会自动删除。要删除所有图像，容器和卷：

```
rm -rf /var/lib/docker
```



### 参考

+ [Centos7.5安装docker（yum安装、rpm安装、脚本安装docker）](https://blog.51cto.com/7603402/2171815)
+ [步骤1：CentOS7.5 安装docker](https://www.jianshu.com/p/3771b155283b)