---
title: LayaAir 引擎示例
date: 2018-06-11 14:35:30
tags: [LayaAir, 游戏开发]
categories: 游戏开发
---
## Sprite——显示图片
`Sprite.loadImage` 可以直接加载外部，也可以从缓冲区读取图片。`Graphics.drawTexture` 绘制纹理。如果使用 `Sprite.loadImage` 显示图集中一张图，则图集必须已被加载。

方法1：使用loadImage

```
var ape:Sprite = new Sprite();
Laya.stage.addChild(ape);
ape.loadImage("url");```

	
`loadImage()` 加载并显示一个图片。功能等同于 `graphics.loadImage` 方法。支持异步加载。 注意：多次调用 `loadImage` 绘制不同的图片，会同时显示。
			
方法2：使用drawTexture

```
Laya.loader.load("url", Handler.create(this, function():void
{
  var t:Texture = Laya.loader.getRes("url");
  var ape:Sprite = new Sprite();
  ape.graphics.drawTexture(t,0,0);
  Laya.stage.addChild(ape);
  ape.pos(200, 0);
}));```

	
+ `load()` 加载资源。加载错误会派发 Event.ERROR 事件，参数为错误信息。
+ `getRes()` 获取指定资源地址的资源。


