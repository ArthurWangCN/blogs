---
title: Mac上安装bower
date: 2017-04-24 21:37:41
tags: 工具
categories: 工具
---
1. 安装bower，得首先安装node：　　
```
brew install npm  //npm是nodejs的程序包管理器，如果安装过nodejs，可忽略此步。
```

2. 安装Git（因为需要从Git仓库获取一些代码包）：
```
sudo brew install git  //也可以安装Git客户端版本
```

3. 安装bower：
```	
sudo npm install -g bower //-g：全局安装
```

4. 配置bower环境变量：
把在第3步提示安装完成的 `bower` 存储路径配置到环境变量中，编辑~/.bash_profile文件：
```	
export GOPATH=/Users/hopkings/www/Go
export GOBIN=$GOPATH/bin
export BOWER=/usr/local/Cellar/node/5.9.1/libexec/npm/lib/node_modules/bower/bin
export PATH=$PATH:$GOBIN:$BOWER:/sbin:/usr/bin:/usr/sbin
//BOWER 是我自己的bower安装好的默认路径。
```
source ~/.bash_profile　　

5. 测试是否成功安装：
```
bower --version		//1.7.9
```

安装成功！