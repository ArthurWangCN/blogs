---
title: 小程序引入icon的三种方式
date: 2020-02-21 17:27:52
tags: 小程序
categories: [前端, 小程序]
---
好的应用需要好的图标来映衬。

使用图片做图标的弊端有：
+ 固定尺寸
+ 激活样式需要做两套
+ 占空间。

相对于图片图标，字体图标完美的解决了这些问题，我们下面介绍小程序里引入图片的三种方式。


## 原生图标
小程序里原生图标是通过 `icon` 标签来引入的：

```
<icon type="success" size="24" color="green" />
```

![原生图标](http://blogpic.at15cm.com/miniprogram-icon-1.jpeg)

但是原生图标只有这么几个，面对前端妖娆繁杂的需求是捉襟见肘，远远不够用啊！

## WeUI图标组件
WeUI 是一套同微信原生视觉体验一致的基础样式库，基于小程序自定义组件构建。由微信官方设计团队为微信内网页和微信小程序量身设计，令用户的使用感知更加统一。

要使用WeUI组件，我们需要到组件下载页面去下载icon组件：[developers.weixin.qq.com/miniprogram/dev/extended/weui/download.html](developers.weixin.qq.com/miniprogram/dev/extended/weui/download.html)

![小程序官网下载weui图标组件](http://blogpic.at15cm.com/miniprogram-icon-2.jpeg)

下载完成后我们会得到一个 `icon` 文件夹。

里面有四个文件：`icon.js, icon.json, icon.wxml,icon.wxss`

我们在项目根目录下创建一个 `components` 文件夹，再将 icon 文件夹拷贝进去，然后在 `app.json` 中增加以下配置：

```
"usingComponents": {
  "mp-icon": "components/icon/icon" //这个地方路径根据具体项目调整
}
```

在测试页面增加一段代码：

```
<mp-icon icon="add" color="white" size="{{25}}"></mp-icon>
```

组件库里 icon 图标列表请参考这个链接：<developers.weixin.qq.com/miniprogram/dev/extended/weui/icon.html>。

目前一共有81个，这些图标对本人来说已经够用了。
但是对于很多特定行业比如旅游，外卖APP来说，图标定制化程度可能更高。
那么我们也需要让小程序有这样引入外部资源的能力。

## iconfont图标
真不是说，阿里的这个图标库([www.iconfont.cn](www.iconfont.cn))真是给众多不会搞设计的程序员带来了莫大的福祉。

上面的图标种类繁多，你想要的都有，更多的还是你想不到的。
你需要做的就是把喜欢的图标加入购物车，然后分类作为一个项目下载下来：

![iconfont图标](http://blogpic.at15cm.com/miniprogram-icon-3.jpeg)

解压下载下来的 `download.zip` 文件，将其中的 `iconfont.css` 文件拷贝到小程序 `utils` 目录下，重命名为 `iconfont.wxss`，打开 `iconfont.wxss`，将 `@font-face` 部分的一些 url 引用(红色框选部分)删除掉：

![iconfont.wxss代码](http://blogpic.at15cm.com/miniprogram-icon-4.jpeg)

打开 `app.wxss` ,在首行引入 `iconfont.wxss`：

```
/**app.wxss**/
@import './utils/iconfont.wxss';
```

在视图文件中，我们通过以下方式显示图标：

```
<icon class="iconfont icon-youxiang"></icon>
```

效果图：

![效果图](http://blogpic.at15cm.com/miniprogram-icon-5.jpeg)


本文来自 [鲲圭云计算-百家号](https://baijiahao.baidu.com/s?id=1651775595545295274&wfr=spider&for=pc)