---
title: WebPack 快速上手
date: 2017-05-03 17:24:40
tags: [webpack, 工具]
categories: 工具
---
## 写在前面的话
>阅读本文之前，先看下面这个 `webpack` 的配置文件，如果每一项你都懂，那本文能带给你的收获也许就比较有限，你可以快速浏览或直接跳过；如果你对很多选项存在着疑惑，那花一段时间慢慢阅读本文，你的疑惑一定一个一个都会消失；如果你以前没怎么接触过Webpack，而你又你对webpack感兴趣，那么动手跟着本文中那个贯穿始终的例子写一次，写完以后你会发现你已明明白白的走进了Webpack的大门。

```
//一个常见的Webpack配置文件
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');
var ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  entry: __dirname + "/app/main.js",
  output: {
    path: __dirname + "/build",
    filename: "[name]-[hash].js"
  },

  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel'
      },
      {
        test: /\.css$/,
        loader: ExtractTextPlugin.extract('style', 'css?modules!postcss')
      }
    ]
  },
  postcss: [
    require('autoprefixer')
  ],

  plugins: [
    new HtmlWebpackPlugin({
      template: __dirname + "/app/index.tmpl.html"
    }),
    new webpack.optimize.OccurenceOrderPlugin(),
    new webpack.optimize.UglifyJsPlugin(),
    new ExtractTextPlugin("[name]-[hash].css")
  ]
}
```

## 什么是WebPack
### 为什要使用WebPack
现今的很多网页其实可以看做是功能丰富的应用，它们拥有着复杂的JavaScript代码和一大堆依赖包。为了简化开发的复杂度，前端社区涌现出了很多好的实践方法

+ 模块化，让我们可以把复杂的程序细化为小的文件;
+ 类似于TypeScript这种在JavaScript基础上拓展的开发语言：使我们能够实现目前版本的JavaScript不能直接使用的特性，并且之后还能能装换为JavaScript文件使浏览器可以识别；
+ Scss，less等CSS预处理器
+ ...

这些改进确实大大的提高了我们的开发效率，但是利用它们开发的文件往往需要进行额外的处理才能让浏览器识别,而手动处理又是非常繁琐的，这就为WebPack类的工具的出现提供了需求。

### 什么是Webpack
`WebPack` 可以看做是模块打包机：它做的事情是，分析你的项目结构，找到 `JavaScript` 模块以及其它的一些浏览器不能直接运行的拓展语言（`Scss，TypeScript等`），并将其打包为合适的格式以供浏览器使用。

### WebPack和Grunt以及Gulp相比有什么特性
其实Webpack和另外两个并没有太多的可比性，`Gulp/Grunt` 是一种能够优化前端的开发流程的工具，而 `WebPack` 是一种模块化的解决方案，不过 Webpack 的优点使得 Webpack 可以替代 Gulp/Grunt 类的工具。

`Grunt` 和 `Gulp` 的工作方式是：在一个配置文件中，指明对某些文件进行类似编译，组合，压缩等任务的具体步骤，这个工具之后可以自动替你完成这些任务。

![Grunt和Gulp的工作流程](http://blogpic.at15cm.com/grunt-gulp.png)

`Webpack` 的工作方式是：把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack 将从这个文件开始找到你的项目的所有依赖文件，使用 `loaders` 处理它们，最后打包为一个浏览器可识别的 `JavaScript` 文件。

![Webpack工作方式](http://blogpic.at15cm.com/webpack-operation.png)

如果实在要把二者进行比较，Webpack的处理速度更快更直接，能打包更多不同类型的文件。


## 开始使用Webpack
初步了解了 Webpack 工作方式后，我们一步步的开始学习使用 Webpack。

### 安装
`Webpack` 可以使用 `npm` 安装，新建一个空的练习文件夹（此处命名为webpack sample progect），在终端中转到该文件夹后执行下述指令就可以完成安装。

```
//全局安装
npm install -g webpack
//安装到你的项目目录
npm install --save-dev webpack
```

### 正式使用Webpack前的准备
1.在上述练习文件夹中创建一个 `package.json` 文件，这是一个标准的 `npm` 说明文件，里面蕴含了丰富的信息，包括当前项目的依赖模块，自定义的脚本任务等等。在终端中使用 `npm init` 命令可以自动创建这个 package.json 文件

```
npm init
```

输入这个命令后，终端会问你一系列诸如项目名称，项目描述，作者等信息，不过不用担心，如果你不准备在npm中发布你的模块，这些问题的答案都不重要，回车默认即可。

2.package.json 文件已经就绪，我们在本项目中安装 Webpack 作为依赖包

```
// 安装Webpack
npm install --save-dev webpack
```

回到之前的空文件夹，并在里面创建两个文件夹, `app` 文件夹和 `public` 文件夹，app 文件夹用来存放原始数据和我们将写的 JavaScript 模块，public 文件夹用来存放准备给浏览器读取的数据（包括使用 webpack 生成的打包后的 js 文件以及一个 index.html 文件）。在这里还需要创建三个文件，`index.html` 文件放在 public 文件夹中，两个js文件（Greeter.js和main.js）放在 app 文件夹中，此时项目结构如下图所示

![项目结构](http://blogpic.at15cm.com/webpack-demo-structure.png)

**index.html** 文件只有最基础的html代码，它唯一的目的就是加载打包后的js文件（bundle.js）

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Webpack Sample Project</title>
  </head>
  <body>
    <div id='root'>
    </div>
    <script src="bundle.js"></script>
  </body>
</html>
```

**Greeter.js** 只包括一个用来返回包含问候信息的html元素的函数。

```
// Greeter.js
module.exports = function() {
  var greet = document.createElement('div');
  greet.textContent = "Hi there and greetings!";
  return greet;
};
```

**main.js** 用来把Greeter模块返回的节点插入页面。

```
//main.js 
var greeter = require('./Greeter.js');
document.getElementById('root').appendChild(greeter());
```

### 正式使用Webpack
webpack可以在终端中使用，其最基础的命令是

```
webpack {entry file/入口文件} {destination for bundled file/存放bundle.js的地方}
```

只需要指定一个入口文件，webpack 将自动识别项目所依赖的其它文件，不过需要注意的是如果你的 webpack 没有进行全局安装，那么当你在终端中使用此命令时，需要额外指定其在 node_modules 中的地址，继续上面的例子，在终端中属于如下命令

```
//webpack非全局安装的情况
node_modules/.bin/webpack app/main.js public/bundle.js
```

结果如下

![终端](http://blogpic.at15cm.com/webpack-demo-result1.png)

可以看出webpack同时编译了main.js 和Greeter,js,现在打开index.html,可以看到如下结果

![页面显示](http://blogpic.at15cm.com/webpack-demo-page1.png)

有没有很激动，已经成功的使用Webpack打包了一个文件了。不过如果在终端中进行复杂的操作，还是不太方便且容易出错的，接下来看看Webpack的另一种使用方法。
