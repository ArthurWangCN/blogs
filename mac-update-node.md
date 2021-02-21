---
title: MAC升级Nodejs和Npm到最新版
date: 2020-02-02 23:47:25
tags: [node, tools]
categories: tools
---
## 第一步，先查看本机node.js版本

```
node -v
```

## 第二步，清除node.js的cache

```
sudo npm cache clean -f
```

## 第三步，安装 n 工具
这个工具是专门用来管理node.js版本的，别怀疑这个工具的名字，是他是他就是他，他的名字就是 "n"

```
sudo npm install -g n
```

## 第四步，安装最新版本的node.js

```
sudo n stable
```

## 第五步，再次查看本机的node.js版本

```
node -v
```

## 第六步，更新npm到最新版

```
$ sudo npm install npm@latest -g
```

## 第七步，验证

```
node -v
npm -v
```

本文来自 [南方帅](https://www.jianshu.com/p/71c82fc63522)
