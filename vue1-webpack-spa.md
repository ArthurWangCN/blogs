---
title: Vue.js 1.0 + Webpack 实现SPA应用文档
date: 2017-04-28 14:15:24
tags: Vue
categories: 前端
---
## 安装webpack
### 首先需要先安装好node.js

### 创建项目并在根目录生成package.json文件

```
mkdir project-name
cd project-name
npm init  //生成package.json文件,一直回车就可以
```

### 安装webpack

```
npm install -g webpack
npm install -g webpack-dev-server  //安装webpack的本地webserver
```

### 安装项目依赖
`webpack` 只能处理 `JavaScript`，因此需要引进其他 `loader` 来处理各种类型的文件，`loader` 可以理解为是模块和资源的转换器，它本身是一个函数，接受源文件作为参数，返回转换的结果。这样，我们就可以通过 `require` 来加载任何类型的模块或文件，比如 `VUE`、`JSX`、`SASS` 或图片。

```
css-loader：编译写入css
style-loader：把编译后的css整合进html
file-loader：编译写入文件，默认情况下生成文件的文件名是文件名与MD5哈希值的组合
vue：vue主程序
vue-laoder：编译写入.vue文件
vue-html-loader：编译vue的template部分
vue-style-loader：编译vue的样式部分
vue-hot-reload-api：webpack对vue实现热替换
babel-core：ES2015编译核心
babel-loader：编译写入ES2015文档
babel-preset-es2015：ES2015语法
babel-preset-stage-0：开启测试功能
babel-runtime：babel执行环境
webpack-merge：开发环境和生产环节的webpaak配置文件的配置合并
```

在命令行中输入：

```
npm install css-loader style-loader --save-dev
```

在 `package.json` 中，主要是 `devDependencies` 这个字段有了改变：

```
"devDependencies": {
    "css-loader": "^0.23.1",
    "style-loader": "^0.13.0",
    "webpack": "^1.12.2"
}
```

我们可以用一个更加方便的方式进行安装，可以直接在 `package.json` 中，添加相应的依赖（如上面的代码），之后的命令行中运行 `npm intall`，它会自动帮我们安装相应的依赖。

`package.json` 的代码如下：

```
{
  "name": "webpackvue",
  "version": "1.0.0",
  "description": "",
  "main": "vue.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.9.1",
    "babel-loader": "^6.2.4",
    "babel-plugin-transform-runtime": "^6.9.0",
    "babel-preset-es2015": "^6.9.0",
    "babel-preset-stage-0": "^6.5.0",
    "babel-runtime": "^6.9.2",
    "css-loader": "^0.23.1",
    "file-loader": "^0.8.5",
    "style-loader": "^0.13.1",
    "url-loader": "^0.5.7",
    "vue": "^1.0.24",
    "vue-router": "^0.7.13",
    "vue-hot-reload-api": "^1.3.2",
    "vue-html-loader": "^1.2.2",
    "vue-loader": "^8.5.2",
    "vue-style-loader": "^1.0.0",
    "webpack": "^1.13.1",
    "webpack-dev-server": "^1.14.1",
    "webpack-merge": "^0.13.0"
  },
  "dependencies": {
    "vue-resource": "^0.9.3"
  }
}
```

在命令行执行 `npm intall` 之后，就会安装我们需要的 `loader` 了。

### 配置webpack.config.js
在根目录下创建 `webpack.config.js`（这个文件需要自己创建的），下面是我的配置


```
var path = require('path');
module.exports = {
  entry: ['./src/main.js'],     //项目入口文件的路径，可以有多个文件，单个文件可以不用中括号
  output: {     //定义webpack输出的文件，这里设置了文件输出在dist文件夹下面的build.js
    path: './dist',
    publicPath:'dist/',
    filename: 'build.js'
  },
  //配置自动刷新,如果打开会使浏览器刷新而不是热替换
  /*devServer: {
    historyApiFallback: true,
    hot: false,
    inline: true,
    grogress: true
  },*/
  module: {
    loaders: [
      //转化ES6语法
      {
        test: /\.js$/,          //这里是匹配文件的正则
        loader: 'babel',        //这里是指定调用loader去处理对应文件类型
        exclude: /node_modules/
      },
      //解析.vue文件
      {
        test:/\.vue$/,
        loader:'vue'
      },
      //图片转化，小于8K自动转化为base64的编码
      {
        test: /\.(png|jpg|gif)$/,
        loader:'url-loader?limit=8192'
      }
    ]
  },
  vue:{
    loaders:{
      js:'babel'
    }
  },
  resolve: {
        // require时省略的扩展名，如：require('app') 不需要app.js
        extensions: ['', '.js', '.vue'],
        // 别名，可以直接使用别名来代表设定的路径以及其他
        alias: {
            filter: path.join(__dirname, './src/filters'),
            components: path.join(__dirname, './src/components')
        }
    }    
}
```

+ entry：指入口文件的配置项，它是一个数组的原因是 webpack 允许多个入口点。 当然如果你只有一个入口的话，也可以直接使用双引号 `"./main.js"`
+ output：配置打包结果，`path` 定义了输出的文件夹，`filename` 则定义了打包结果文件的名称
+ module：定义了对模块的处理逻辑，这里可以用 `loaders` 定义了一系列的加载器，以及一些正则。当需要加载的文件匹配 `test` 的正则时，就会调用后面的 `loader` 对文件进行处理，这正是webpack强大的原因。

注意：配置 `babel` 需要创建 `.babelrc`，不可以新建文本 `.txt` 再该后缀，应该通过命令行 `cd.>.babelrc` 新建该文件，再打开文件写入

```
{ "presets": ["es2015", "stage-0"], "plugins": ["transform-runtime"] }
```

### 创建项目目录
在根目录新建 `src` 文件夹，在 `src` 里新建 `main.js`，此时我们的目录结构如下：

```
|        |-src
|        |        |-main.js
|        |-node_modules
|        |-webpack.config.js
|        |-package.json
|        |-.babelrc
```

## 使用webpack
1.配置好webpack.config.js之后，命令行执行

```
webpack
```

则构建成功了，entry入口文件都压缩到指定目录下生成一个文件build.js，index.html引入build即可。然后可以打开index.html看效果。 （每次修改文件之后，都需要再次执行此命令。）

2.webpack-dev-server，一个轻量的node.js express服务器，webpack-dev-server支持两种模式的自动刷新页面：

iframe模式（页面嵌入一个iframe并在其中呈现页面的变化）
inline模式（一个小型的webpack-dev-server客户端会作为入口文件打包，这个客户端会在后端代码改变的时候刷新页面）

使用iframe模式无需额外的配置，我们可以通过执行命令行

```
webpack-dev-server
```

启动服务器之后，在浏览器输入 `http://localhost:8080`，则可以打开项目了。每次修改完代码只需要刷新浏览器则可。
使用inline模式，我们可以通过执行命令行
```
webpack-dev-server --inline --hot
```
修改完在代码之后，不用刷新就可以看到项目改变了。（然而并没什么卵用，data 里面的值是初始值，热更新也不会改变他们，只能刷新。）


## 学习Vue.js 1.0
1. 官网：<http://cn.vuejs.org/guide/>
2. segmentfault：<https://segmentfault.com/>
3. vue-router 1.0中文文档：<http://router.vuejs.org/>


## 使用Vue.js实现SPA应用
（注意：vue-router2的写法已经不同）
在我们主要的main.js里

```
import Vue from 'vue';                    //引入vue
import VueResource from 'vue-resource';   //引入vue-resource,用于请求数据
import VueRouter from 'vue-router';       //引入vue-router,路由
import App from './app.vue';              //下面这几个是我们自己写的组件，全部都在main.js引进来
import home from './components/home.vue';
import list from './components/list.vue';
import hello from './components/hello.vue';
Vue.use(VueRouter);
Vue.use(VueResource);
var app = Vue.extend(App);
var router = new VueRouter();
//配置路由
router.map({
    '/home':{
        component:home
    },
    '/list':{
        component:list
    },
    '/hello':{
        component:hello
    }
})
//配置路由默认进入的页面
router.redirect({
    '/':'home'
})
router.start(app,'#app');
window.router = router;
```

这样我们路由就配置成功了，在浏览器访问 <http://localhost:8080> 就可以访问到项目的home页面了。

## 参考文献
+ [Vue.js 1.0 + Webpack 实现SPA应用文档](http://www.qdfuns.com/notes/15904/cad84afddab13604f987cbf511d265e4.html)