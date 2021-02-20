---
title: Vue学习笔记（四）模块化
date: 2017-04-27 09:14:02
tags: Vue
categories: 前端
---
## 前言
>`node.js` 天生就是模块化，可以 `require`，可以 `exports`，后天可以模块化，前台为什么不能实现模块化呢？于是出现了 `broserify`，它是一个模块加载器，但是只能加载 JS ，于是 `webpack` 出现了，它的理念是 `一切东西都是模块`，最后打包一块。基于 `webpack` 的 `vue-loader` 概念来源于其他的loader: css-loader,url-loader,html-loader。

### .vue文件
`.vue` 文件放置的是 `vue` 组件或模块的代码，具体：

```
<template>
	html
</template>
<style>
	css
</style>
<script>
	js	(平时代码/ES6)->ES6目前浏览器支持的不太好，所以要用babel编译成ES5
</script>
```

### 简单的目录结构
#### 目录结构
+ 新建文件夹 `vue-loader-demo`
+ 目录结构

```
|-index.html
|-main.js	入口文件
|-App.vue	vue文件
|-package.json	工程文件（项目依赖、名称、配置）
|-webpack.config.js		webpack配置文件
```

#### 生成 packge.json 文件
终端 `cd` 到 `vue-loader-demo`，执行命令：

```
npm init --yes
```

![package.json](http://blogpic.at15cm.com/vue-loader-1.png)


## ES6模块化开发
**导出模块：**

```
export default {}
```

**引入模块：**

```
import 模块名 from 地址
```


## webpack准备工作
>WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。

>webpack-dev-server是一个小型的 `Node.js Express` 服务器,它使用 `webpack-dev-middleware` 来服务于 `webpack` 的包,除此自外，它还有一个通过 `Sock.js` 来连接到服务器的微型运行时.

**通过npm安装webpack和webpack-dev-server:**

```
npm install webpack --save-dev
npm install webpack-dev-server --save-dev
```

这里 `--save-dev` 会写进 `package.json` 中的 `devDependencies`


## App.vue编译为正常代码

`App.vue` 编译为正常代码需要 `vue-loader`，下载：

```
npm install vue-loader@8.5.4 --save-dev
```

解析 `App.vue` 中的代码需要 `vue-html-loader`：

```
npm install vue-html-loader --save-dev
npm install vue-style-loader --save-dev
npm install vue-hot-reload-api --save-dev
```

webpack在安装 `xxx-loader` 的时候会报错，这些东西应该都是 `vue-loader` 的依赖，[解决方法](https://segmentfault.com/q/1010000005651509?sort=created)


项目具体见：[Vue.js 1.0 + Webpack 实现SPA应用文档](http://legendaryarthur.cn/2017/04/28/vue1-webpack-spa/)


## 脚手架
>实际开发中不会自己一点点的配，可选用脚手架 `vue-cli` 帮你提供好基本项目结构。vue-cli 是vue.js的脚手架，用于自动生成vue.js模板工程的。

本身集成了很多项目模板：

+ simple
没有实际用处
+ webpack
推荐大型项目使用，Eslint检查代码规范，单元测试
+ webpack-simple
个人推荐，没有代码检查
+ browerify
+ browserify-simple

### 基本使用流程
安装vue-cli之前，需要先安装了vue和webpack
安装 vue 命令环境

```
npm install -g vue-cli		//全局安装vue-cli
vue init webpack projectName		//生成项目名为projectName的模板，这里的项目名projectName随你自己写
cd projectName                              
npm install		//初始化安装依赖
```

这样子项目就安装完了。生成的项目下面的目录是这样的

![项目目录](http://blogpic.at15cm.com/vue-cli-demo.png)

然后执行

```
npm run dev
```               

在浏览器打开 <http://localhost:8080>，则可以看到欢迎页了。

![欢迎页](http://blogpic.at15cm.com/vue-cli-home.png)

但是这个只能在本地跑，要如何在我们自己的服务器上访问呢？此时需要执行

```
npm run build
```

会生成静态文件，在根目录的 `dist` 里，里面有个 `index.html`，这是服务器访问的路径指定到这里就可以访问我们自己的项目了。但是我发现个问题就是生成 `index.html` 里引用的 `css` 和 `js` 的引用路径不对，这时候就需要自己修改一下配置了。

进入 `config/index.js`

原来的配置的引用路径为：

```
assetsPublicPath: '/',
```

修改：

```
assetsPublicPath: './',
```

参考连接：[Vue.js：使用vue-cli快速构建项目](http://www.qdfuns.com/notes/15904/fbb4d15b9c22fd373b605805bde8fd44.html)


```
#Strive讲解
1.npm install vue-cli -g	安装vue命令环境
	验证安装完成->vue --version
2.生成项目模板
	vue init <模板名> 本地文件夹名称
3.进入到生成目录里面
	cd xxx
	npm install
4.npm run dev 
```
