---
title: Vue学习笔记（七） Element UI 及 Mint UI
date: 2017-05-01 17:21:23
tags: Vue
categories: 前端
---
>`Element UI` 是 饿了么 前端团队推出的一款基于 `Vue 2.0` 的桌面端UI框架，手机端有对应框架是 `Mint UI` 。

## Element UI
>一套为开发者、设计师和产品经理准备的基于 `Vue 2.0` 的组件库，提供了配套设计资源，帮助你的网站快速成型。

### 安装
#### npm 安装

推荐使用 `npm` 的方式安装，它能更好地和 `webpack` 打包工具配合使用。

```
npm i element-ui -S
```

#### CDN
目前可以通过 `unpkg.com/element-ui` 获取到最新版本的资源，在页面上引入 `js` 和 `css` 文件即可开始使用。

```
<!-- 引入样式 -->
<link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-default/index.css">
<!-- 引入组件库 -->
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
```

#### Hello world
通过 `CDN` 的方式我们可以很容易地使用 `Element` 写出一个 `Hello world` 页面。

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <!-- 引入样式 -->
  <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-default/index.css">
</head>
<body>
  <div id="app">
    <el-button @click="visible = true">按钮</el-button>
    <el-dialog v-model="visible" title="Hello world">
      <p>欢迎使用 Element</p>
    </el-dialog>
  </div>
</body>
  <!-- 先引入 Vue -->
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
  <!-- 引入组件库 -->
  <script src="https://unpkg.com/element-ui/lib/index.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: function() {
        return { visible: false }
      }
    })
  </script>
</html>
```

### 快速上手
本节将介绍如何在项目中使用 Element。

#### 使用 Starter Kit
我们提供了通用的项目模板，你可以直接使用。对于熟悉 `cooking` 或 `Laravel` 的用户，我们也准备了相应的模板，同样可以直接下载使用。

如果不希望使用我们提供的模板，请继续阅读。

#### 配置文件

新建项目，项目结构为

```
|- src/  --------------------- 项目源代码
    |- App.vue
    |- main.js  -------------- 入口文件
|- .babelrc  ----------------- babel 配置文件
|- index.html  --------------- HTML 模板
|- package.json  ------------- npm 配置文件
|- README.md  ---------------- 项目帮助文档
|- webpack.config.js  -------- webpack 配置文件
```

几个配置文件的典型配置如下：

**.babelrc**

```
{
  "presets": [
    ["es2015", { "modules": false }]
  ]
}
```

**package.json**

```
{
  "name": "element-starter",
  "scripts": {
    "dev": "cross-env NODE_ENV=development webpack-dev-server --inline --hot --port 8086",
    "build": "cross-env NODE_ENV=production webpack --progress --hide-modules"
  },
  "dependencies": {
    "element-ui": "^1.0.0",
    "vue": "^2.1.6"
  },
  "devDependencies": {
    "babel-core": "^6.0.0",
    "babel-loader": "^6.0.0",
    "babel-preset-es2015": "^6.13.2",
    "cross-env": "^1.0.6",
    "css-loader": "^0.23.1",
    "file-loader": "^0.8.5",
    "style-loader": "^0.13.1",
    "vue-loader": "^9.8.0",
    "webpack": "beta",
    "webpack-dev-server": "beta"
  }
}
```

**webpack.config.js**

```
var path = require('path')
var webpack = require('webpack')

module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    filename: 'build.js'
  },
  module: {
    loaders: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      {
        test: /\.css$/,
        loader: 'style-loader!css-loader'
      },
      {
        test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
        loader: 'file-loader'
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?\S*)?$/,
        loader: 'file-loader',
        query: {
          name: '[name].[ext]?[hash]'
        }
      }
    ]
  },
  devServer: {
    historyApiFallback: true,
    noInfo: true
  },
  devtool: '#eval-source-map'
}

if (process.env.NODE_ENV === 'production') {
  module.exports.devtool = '#source-map'
  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      }
    })
  ])
}
```


#### 引入 Element
你可以引入整个 `Element`，或是根据需要仅引入部分组件。我们先介绍如何引入完整的 `Element`。

##### 完整引入
在 `main.js` 中写入以下内容：

```
import Vue from 'vue'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'
import App from './App.vue'
Vue.use(ElementUI)
new Vue({
  el: '#app',
  render: h => h(App)
})
```

以上代码便完成了 `Element` 的引入。需要注意的是，样式文件需要单独引入。

##### 按需引入
借助 [babel-plugin-component](https://github.com/QingWei-Li/babel-plugin-component)，我们可以只引入需要的组件，以达到减小项目体积的目的。

首先，安装 `babel-plugin-component：

```
npm install babel-plugin-component -D
```

然后，将 `.babelrc` 修改为：

```
{
  "presets": [
    ["es2015", { "modules": false }]
  ],
  "plugins": [["component", [
    {
      "libraryName": "element-ui",
      "styleLibraryName": "theme-default"
    }
  ]]]
}
```

接下来，如果你只希望引入部分组件，比如 `Button` 和 `Select`，那么需要在 `main.js` 中写入以下内容：

```
import Vue from 'vue'
import { Button, Select } from 'element-ui'
import App from './App.vue'
Vue.component(Button.name, Button)
Vue.component(Select.name, Select)
/* 或写为
 * Vue.use(Button)
 * Vue.use(Select)
 */
new Vue({
  el: '#app',
  render: h => h(App)
})
```

完整组件列表和引入方式（完整组件列表以 `components.json` 为准）

```
import Vue from 'vue'
import {
  Pagination,
  Dialog,
  Autocomplete,
  Dropdown,
  DropdownMenu,
  DropdownItem,
  Menu,
  Submenu,
  MenuItem,
  MenuItemGroup,
  Input,
  InputNumber,
  Radio,
  RadioGroup,
  RadioButton,
  Checkbox,
  CheckboxGroup,
  Switch,
  Select,
  Option,
  OptionGroup,
  Button,
  ButtonGroup,
  Table,
  TableColumn,
  DatePicker,
  TimeSelect,
  TimePicker,
  Popover,
  Tooltip,
  Breadcrumb,
  BreadcrumbItem,
  Form,
  FormItem,
  Tabs,
  TabPane,
  Tag,
  Tree,
  Alert,
  Slider,
  Icon,
  Row,
  Col,
  Upload,
  Progress,
  Spinner,
  Badge,
  Card,
  Rate,
  Steps,
  Step,
  Carousel,
  Scrollbar,
  CarouselItem,
  Collapse,
  CollapseItem,
  Cascader,
  ColorPicker,
  Loading,
  MessageBox,
  Message
} from 'element-ui'
Vue.use(Pagination)
Vue.use(Dialog)
Vue.use(Autocomplete)
Vue.use(Dropdown)
Vue.use(DropdownMenu)
Vue.use(DropdownItem)
Vue.use(Menu)
Vue.use(Submenu)
Vue.use(MenuItem)
Vue.use(MenuItemGroup)
Vue.use(Input)
Vue.use(InputNumber)
Vue.use(Radio)
Vue.use(RadioGroup)
Vue.use(RadioButton)
Vue.use(Checkbox)
Vue.use(CheckboxGroup)
Vue.use(Switch)
Vue.use(Select)
Vue.use(Option)
Vue.use(OptionGroup)
Vue.use(Button)
Vue.use(ButtonGroup)
Vue.use(Table)
Vue.use(TableColumn)
Vue.use(DatePicker)
Vue.use(TimeSelect)
Vue.use(TimePicker)
Vue.use(Popover)
Vue.use(Tooltip)
Vue.use(Breadcrumb)
Vue.use(BreadcrumbItem)
Vue.use(Form)
Vue.use(FormItem)
Vue.use(Tabs)
Vue.use(TabPane)
Vue.use(Tag)
Vue.use(Tree)
Vue.use(Alert)
Vue.use(Slider)
Vue.use(Icon)
Vue.use(Row)
Vue.use(Col)
Vue.use(Upload)
Vue.use(Progress)
Vue.use(Spinner)
Vue.use(Badge)
Vue.use(Card)
Vue.use(Rate)
Vue.use(Steps)
Vue.use(Step)
Vue.use(Carousel)
Vue.use(Scrollbar)
Vue.use(CarouselItem)
Vue.use(Collapse)
Vue.use(CollapseItem)
Vue.use(Cascader)
Vue.use(ColorPicker)
Vue.use(Loading.directive)
Vue.prototype.$loading = Loading.service
Vue.prototype.$msgbox = MessageBox
Vue.prototype.$alert = MessageBox.alert
Vue.prototype.$confirm = MessageBox.confirm
Vue.prototype.$prompt = MessageBox.prompt
Vue.prototype.$notify = Notification
Vue.prototype.$message = Message
```


至此，一个基于 Vue 和 Element 的开发环境已经搭建完毕，现在就可以编写代码了。启动开发模式：

```
//执行如下命令后访问 localhost:8086
npm run dev
```

编译：

```
npm run build
```

各个组件的使用方法请参阅 [Element UI 官方文档](http://element.eleme.io/#/zh-CN/component/installation)，类似 `bootstrap`，没有什么难度。



## Mint UI
本文将介绍 Mint UI 的安装方式和基本的用法。

### 安装
#### npm 安装
推荐使用 `npm` 的方式安装，它能更好地和 `webpack` 打包工具配合使用。

```
npm i mint-ui@1 -S
```

#### CDN
目前可以通过 `unpkg.com/mint-ui@1` 获取到最新版本的资源，在页面上引入 `js` 和 `css` 文件即可开始使用。

```
<!-- 引入样式 -->
<link rel="stylesheet" href="https://unpkg.com/mint-ui@1/lib/style.css">
<!-- 引入组件库 -->
<script src="https://unpkg.com/mint-ui@1/lib/index.js"></script>
```

#### Hello world
通过 `CDN` 的方式我们可以很容易地使用 `Mint UI` 写出一个 `Hello world` 页面。

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <!-- 引入样式 -->
  <link rel="stylesheet" href="https://unpkg.com/mint-ui@1/lib/style.css">
</head>
<body>
  <div id="app">
    <mt-button @click.native="handleClick">按钮</mt-button>
  </div>
</body>
  <!-- 先引入 Vue -->
  <script src="https://unpkg.com/vue@1/dist/vue.js"></script>
  <!-- 引入组件库 -->
  <script src="https://unpkg.com/mint-ui@1/lib/index.js"></script>
  <script>
    new Vue({
      el: '#app',
      methods: {
        handleClick: function() {
          this.$toast('Hello world!')
        }
      }
    })
  </script>
</html>
```

### 快速上手
本节将介绍如何在项目中使用 `Mint UI`。

#### 使用 vue-cli

```
npm install -g vue-cli
vue init webpack projectname
```

#### 引入 Mint UI
你可以引入整个 `Mint UI`，或是根据需要仅引入部分组件。我们先介绍如何引入完整的 `Mint UI`。

##### 完整引入
在 `main.js` 中写入以下内容：

```
import Vue from 'vue'
import MintUI from 'mint-ui'
import 'mint-ui/lib/style.css'
import App from './App.vue'
Vue.use(MintUI)
new Vue({
  el: '#app',
  components: { App }
})
```

以上代码便完成了 `Mint UI` 的引入。需要注意的是，样式文件需要单独引入。

##### 按需引入

借助 `babel-plugin-component`，我们可以只引入需要的组件，以达到减小项目体积的目的。

首先，安装 `babel-plugin-component`：

```
npm install babel-plugin-component -D
```

然后，将 `.babelrc` 修改为：

```
{
  "presets": [
    ["es2015", { "modules": false }]
  ],
  "plugins": [["component", [
    {
      "libraryName": "mint-ui",
      "style": true
    }
  ]]]
}
```

如果你只希望引入部分组件，比如 `Button` 和 `Cell`，那么需要在 `main.js` 中写入以下内容：

```
import Vue from 'vue'
import { Button, Cell } from 'mint-ui'
import App from './App.vue'
Vue.component(Button.name, Button)
Vue.component(Cell.name, Cell)
/* 或写为
 * Vue.use(Button)
 * Vue.use(Cell)
 */
new Vue({
  el: '#app',
  components: { App }
})
```

#### 开始使用
至此，一个基于 `Vue` 和 `Mint UI` 的开发环境已经搭建完毕，现在就可以编写代码了。启动开发模式：

```
npm run dev
```

编译：

```
npm run build
```

各个组件的使用方法请参阅 [Mint UI 官方文档](http://mint-ui.github.io/docs/#/zh-cn)。

