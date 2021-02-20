---
title: LayaAir引擎发布大游戏，支持JS分包加载达8M！
date: 2018-06-20 10:15:10
tags: [游戏开发, LayaAir]
categories: 游戏开发
---
LayaAir引擎最有优势的领域是大型游戏和3D游戏。然而微信小游戏不支持动态加载JS，且在2.1.0版本（基础库）之前的初始包仅支持4M。导致JS超过4M的大型项目必须删减项目内容，才可以发布成微信小游戏。**从2.1.0版本开始，通过分包加载的方式，小游戏总包可增加至8M**。这将极大的满足了2D与3D大型游戏的项目开发与发布需求。

本篇围绕如何在发布大型游戏到微信小游戏的时候，尽可能不受JS包体限制的方式，包括小游戏自带的分包方式与引擎中如何减小JS的经验进行分享，希望能给开发者带来帮助。

## 小游戏分包功能的简介
在小游戏首次启动时先下载的包，称为主包。项目根目录的 `game.js` 为游戏的主包入口。LayaAirIDE发布的小游戏项目，该入口会引用两个JS,一个是小游戏适配库JS，另一个是游戏的真正入口JS。

如果我们的项目JS不超过4M时，其实可以不使用小游戏自带的分包功能。直接使用引擎自带的动态加载再配合缓存也是没问题的。如果项目JS超过4M，使用小游戏的分包，就成为必须了。因为小游戏的分包加载，可以加载JS文件，而通的URL动态加载的，只能是资源和配置文件。

分包是通过根目录的 `game.json` 中配置 `subpackages` 字段来实现的，字段内的目录或 js 文件，将按照配置在上传小游戏微信后打包成一个个“分包”，没有配置在 subpackages 中的目录和 js，将会被打包到主包中。而单个分包或主包大小不能超过 4M，整个小游戏所有分包大小不超过 8M。

在老版本的兼容性方面。由微信后台编译来处理旧版本客户端的兼容，后台会编译两份代码包，一份是分包后代码，另外一份是整包的兼容代码。对于老客户端，会去下载整包代码启动。

## 小游戏分包的具体配置
配置到subpackages字段内的才会按照配置打成一个个的分包。所以只需在subpackages内配置一个一个的name和root即可。name用于加载分包时调用，按自己习惯命名即可。root内可以是目录路径，也可以是具体的js路径。

以下是小游戏官方的 game.json 配置示例：

```
{
  ...
  subpackages: [
    {
      name: 'stage1',
      root: 'stage1/' // 可以指定一个目录，目录根目录下的 game.js 会作为入口文件，目录下所有资源将会统一打包
    }, {
      name: 'stage2',
      root: 'stage2.js' // 也可以指定一个 JS 文件
    }
  ]
  ...
}```

笔者在实测后，除了主包外，只分了一个包（并非是建议，只是与官方示例有所区别），如下图所示：

![分包](http://blogpic.at15cm.com/minigames8m-1.jpg)

需要提醒的是，如果分包的root路径配置为目录的话，该目录必需要有一个game.js作为入口文件，否则会报错。



## 分包加载API的示例代码

通过 `API wx.loadSubpackage()` 来触发分包的下载与加载，调用 `wx.loadSubpackage` 并加载完成后，通过 `wx.loadSubpackage` 的 success 回调来通知加载完成。同时，`wx.loadSubpackage` 会返回一个 `LoadSubpackageTask`，可在其中获取当前下载进度。

小游戏官方示例代码：

```
const loadTask = wx.loadSubpackage({
  name: 'stage1', // name 可以填 name 或者 root
  success: function(res) {    // 分包加载成功后通过 success 回调
  },
  fail: function(res) {    // 分包加载失败通过 fail 回调
  }
})

loadTask.onProgressUpdate(res => {
  console.log('下载进度', res.progress)  
  console.log('已经下载的数据长度', res.totalBytesWritten)  
  console.log('预期需要下载的数据总长度', res.totalBytesExpectedToWrite)
})```

Tips: 开发者在基础库 2.1.0 以下的版本不需要调用 wx.loadSubpackage 触发加载，2.1.0 以下版本不存在 wx.loadSubpackage 方法。



## LayaAir引擎的UI模式
在HTML5项目的开发，通常采用默认的UI模式（内嵌模式），而微信小游戏基于包体大小以及JS不能URL动态加载的限制。了解加载模式与分离模式就比较有意义了。

内嵌模式是指在LayaAirIDE编辑好UI后，导出时，针将UI页面的配置信息等敢出为项目的JS文件，这样无需编写额外的加载代码，即可使用。相对来说比较方便。所以也是默认的UI导出类型。

加载模式与分离模式都是将配置信息导出为json格式的文件，这样，对于小游戏而言，通过加载json文件的方式加载使用，就可以节省大量的JS文件包大小。加载模式与分离模式的区别是，加载模式会将所有的UI页面配置信息统一导出为一个json文件，而分离模式则是每一个页面都会独立导出json文件，按需要进行加载。

如果需要修改页面默认的UI导出模式，可以按F9，在项目设置面板里，在UI模式的选项里进行选择，点击确定进行保存，如下图所示：

![UI模式](http://blogpic.at15cm.com/minigames8m-2.jpg)

或者选中某个页面后，在项目管理器的属性设置里，修改该页面的导出类型。如下图所示：

![UI模式](http://blogpic.at15cm.com/minigames8m-3.jpg)

除了以上的方式，通过IDE发布小游戏时勾选压缩混淆JS选项目，也可以减小JS的初始包大小，另外，对于发布后没有用到的JS引擎库进行删除，也可以减少初始包的大小。


<br/><br />

本文来自 [LayaBox官方公众号](https://mp.weixin.qq.com/s/hXek9clT5u_unDdEWnmQkA)

