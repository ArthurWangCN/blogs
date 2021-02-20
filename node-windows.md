---
title: Windows 下 Node.js开发环境的搭建
date: 2017-04-22 12:09:03
tags: node
categories: 后台
---
>Windows 是一个优秀的桌面操作系统，但是肯定算不上是一个优秀的服务器操作系统。在生产环境中 node.js 大多部署在 linux 上，做开发的时候应该将应用部署在 linux 上，建议将 node.js 运行在 linux 上。


## 合适的开发环境
+ 生产环境中的 node.js 应用
+ Windows + Linux


## 安装
+ VirtualBox
+ 虚拟机 CentOS 安装
+ Xshell 与 Xftp
+ Node.js
+ MongoDB
+ Redis
+ Sublime Text
+ WebStorm


### VirtualBox
#### 简介
VirtualBox 是一款开源虚拟机软件。

VirtualBox 是由德国 Innotek 公司开发，由Sun Microsystems公司出品的软件，使用Qt编写，在 Sun 被 Oracle 收购后正式更名成 Oracle VM VirtualBox。

Innotek 以 GNU General Public License (GPL) 释出 VirtualBox，并提供二进制版本及 OSE 版本的代码。
使用者可以在VirtualBox上安装并且执行Solaris、Windows、DOS、Linux、OS/2 Warp、BSD等系统作为客户端操作系统。

现在则由甲骨文公司进行开发，是甲骨文公司xVM虚拟化平台技术的一部份。

#### 下载
下载地址：[VirtualBox](https://www.virtualbox.org/wiki/Downloads)

#### 安装
安装过程相当简单，按照步骤一直装下去就好了

#### 使用
1. 新建虚拟电脑
点击 `新建`，名称为 `Node.js`，类型选择 `Linux`，版本选择 `Other Linux(64-bit)`
2. 设置内存
设置 `1024MB`
3. 创建硬盘
选择默认格式，为了节省物理机磁盘空间，我们选择 `动态分配`
4. 文件位置和大小
默认


### CentOS
#### 简介
CentOS（Community Enterprise Operating System，社区企业操作系统）

CentOS 是 linux 系统的一个发行版，也就是 linux 系统中的一个。

它是基于 linux 红帽版本制作的，红帽版因为是商业版，所以很多东西是要钱的，但是 CentOS 完全免费，

主要用作服务器的搭建。

#### 下载
下载地址：[CentOS](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1611.iso)

#### 为虚拟机配置镜像
`设置` -> `存储` -> `光驱` -> `选择一个虚拟光盘` -> 找到镜像所在的位置打开

#### 网络设置
>默认情况下的连接方式是 `网络地址转换(NAT)`，也就是说虚拟机通过物理机的网络进行访问，这样的话没办法在物理机上通过 `SSH` 来连接虚拟机

如果是通过路由器上网，则可以选择 `桥接网卡`，这样的话虚拟机和物理机在路由器里是对准的两个节点，这样我们就可以在物理机上通过SSH来实现对虚拟机的访问和控制。

#### 安装虚拟机
虚拟机和物理机之间鼠标的切换是通过键盘右侧的 `Ctrl`。

1. 使用键盘上下键选择 `Install CentOS 7`，然后按 `Enter` 键；
2. 语言：尽量不要选择中文，最好为默认；
3. 点击 `SOFTWARE SELECTION`，左侧选择 `Basic Web Server`，右侧扩展包选择 `Development Tools`；
4. 选择安装的硬盘，点击 `INSTALLATION DESTINATION`，这里只有一块硬盘。虽然已经勾选，但是要先取消，再勾选；
5. 点击安装 `Begin Installation`；
6. 设置管理员密码，点击 `ROOT PASSWORD`，填写密码即可；
7. 等待安装完成即可，整个过程大概3-5分钟。

### 启动虚拟机
1. 完成后点击 `reboot` 重启，登录时填写你的用户名(root)和密码；
2. 修改虚拟机的网络：
```
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
```
CentOS 7 默认情况下启动的时候网卡是关闭的，使用 vi 命令把 `ONBOOT=no` 改为 `ONBOOT=yes`

重启虚拟机的网络：
```
systemctl restart network
```

查看网卡是否分配到了IP地址：
```
ifconfig
```
![查看是否分配到IP地址](http://blogpic.at15cm.com/node170422-1.png)

查看网络的连接性：
```
ping www.baidu.com
```
有数据说明连接到了外部网络，可以用 `Ctrl+c` 停止

平时使用过程中可能会有虚拟机的IP地址变化，导致要修改一大堆文件。为了防止这种现象发生，在物理机上为虚拟机映射一个域名。
打开物理机的hosts文件，路径：`c:\Windows\System32\drivers\ect`，添加内容如下：
```
192.168.5.14    geek
```
保存（需要管理员权限）


3. 





### Xshell
#### 简介
Xshell 是一个强大的安全终端模拟软件，它支持 SSH1, SSH2, 以及Microsoft Windows 平台的TELNET 协议。Xshell 通过互联网到远程主机的安全连接以及它创新性的设计和特色帮助用户在复杂的网络环境中享受他们的工作。

Xshell 可以在 Windows 界面下用来访问远端不同系统下的服务器，从而比较好的达到远程控制终端的目的。

#### 下载
填写简单的信息就可以获取下载地址

下载地址：[Xshell 5 Download](http://www.netsarang.com/download/down_form.html?code=522)
 
#### 安装
按照步骤一步步安装下去即可，注意刚开始许可类型选择 `免费为家庭/学校` 。

#### 使用
`新建回话属性` -> 名称：`geek_node.js` -> 协议：`SSH` -> 主机：`geek` -> 点击 `连接` -> 输入用户名和密码

至此，虚拟机的安装完成。

###Xftp
#### 简介
Xftp是一个基于 MS windows 平台的功能强大的 SFTP、FTP 文件传输软件。

使用了 Xftp 以后，MS windows 用户能安全地在 UNIX/Linux 和 Windows PC 之间传输文件。

Xftp 能同时适应初级用户和高级用户的需要。它采用了标准的 Windows 风格的向导，它简单的界面能与其他 Windows 应用程序紧密地协同工作，此外它还为高级用户提供了众多强劲地功能特性。

####下载
填写简单的信息就可以获取下载地址

下载地址：[Xftp 5 Download](http://www.netsarang.com/download/down_form.html?code=523)

#### 安装
按照步骤一步步安装下去即可。


### 安装EPEL
EPEL 是yum的一个软件源，里面包含了许多基本源里没有的软件。

安装:
```
yum install epel-release
```

### 安装node.js 
```
yum install nodejs
```

检测node.js是否安装成功（node版本）：
```
node --version
```

### MongoDB
#### 安装MongoDB服务器端
```
yum install mongodb-server
```

#### 安装MongoDB客户端
```
yum install mongodb
```

### Redis
[Redis](http://www.runoob.com/redis/redis-intro.html) 是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。
从2010年3月15日起，Redis的开发工作由VMware主持。

安装：
```
yum install redis
```


## 运行第一个程序
创建一个文件：
```
vim test.js
```

使用vim指令写入内容：
```
console.log('hi,node');
```

用node来运行：
```
node test.js
```



