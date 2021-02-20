---
title: 从NPM到CNPM
date: 2017-04-27 17:10:09
tags: [工具, npm]
categories: 工具
---
NPM是Nodejs的包管理工具，目前 NPM 社区包的数量已超越 C、C++，已然成为全球最大的代码工厂;

安装 Nodejs 后即可开始 NPM 之旅了，新建一个 `package.json` 或者通过 `npm init`，来更好的为 NPM 服务；
配置 `package.json` 的 `dependencies` 属性和 `devDependencies` 属性，指定生产环境和开发环境所需依赖的包，命令行 `npm install` 即可全部安装；
或者 `npm install -g moduleName` 来全局安装某个模块，`npm install --save moduleName` 安装生产环境所需的包，`npm install --dev moduleName` 安装开发环境所需的包；


**其他常用命令：**

```
npm update/uninstall moduleName更新或卸载某个包；
npm list查看当前目录下已安装的包；
npm root -g查看全局安装的包的路径；
npm help查看全部命令；
```

有了 `Browserify` 后，你能做的更多了。曾几何时，javascript由于被限定在浏览器内，做什么都扯手扯脚，一度被开发者们不认可；而现在javascript倍受追捧，漂亮的逆袭了，还能自由的游走于前后端，我想，`Nodejs` 无疑发挥着历史性的作用。

在前端，只要你按照 Nodejs 模块化的方式开发，即可同样的调用相应的 Nodejs 内部和外部模块，由 Browserify 帮你处理依赖，一并打包为前端可调用的js文件；同样在Nodejs里，你可以 require 前端编写的符合Nodejs模块化方式的模块；从而，很容易一步步构建基于Nodejs的前端工程化体系，并且前后端可以共用一套；

阿里的前辈们一直在为人民谋福利；无论你有没有“被墙”，阿里的福利就在这： [淘宝 NPM 镜像](https://npm.taobao.org/) ；这是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步；你可以使用cnpm命令行工具替代默认的NPM；还有很多镜像，包括对于Nodejs你所需要的众多重要信息资料；


**使用cnpm替代默认的npm：**

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

或者直接通过添加 npm 参数 `alias` 一个新命令：

```
alias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"
```

`# Or alias it in .bashrc or .zshrc`

```
$ echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"'
```

OK，下面你就可以通过 `cnpm install moduleName` 来像使用npm一样安装你所需的包了；所有包都可以在这找到全部信息，所以，你懂的，大大的福利！


**参考文献：**

+ [从NPM到CNPM](http://www.tuicool.com/articles/mmYZBn)
+ [给电脑换源 npm 国内镜像 cnpm](http://yijiebuyi.com/blog/b12eac891cdc5f0dff127ae18dc386d4.html)