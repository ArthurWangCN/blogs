---
title: AS3：理解Stage
date: 2018-06-04 18:30:45
tags: [游戏开发, AS3]
categories: 游戏开发
---
Stage 类代表主绘图区。 舞台表示显示 Flash ® 内容的整个区域。
无法以全局方式访问 Stage 对象。 而是需要利用 DisplayObject 实例的 stage 属性进行访问。

## stage对齐: align
一个 StageAlign 类中指定舞台在 Flash Player 或浏览器中的对齐方式的值。 以下是有效值：

|值|垂直对齐方式|水平对齐方式|
|:---|:--------|:---------|
|StageAlign.TOP	|顶对齐	|居中对齐|
|StageAlign.BOTTOM	|底对齐	|居中对齐|
|StageAlign.LEFT	|居中对齐	|左对齐|
|StageAlign.RIGHT	|居中对齐	|右对齐|
|StageAlign.TOP_LEFT	|顶对齐	|左对齐|
|StageAlign.TOP_RIGHT	|顶对齐	|右对齐|
|StageAlign.BOTTOM_LEFT	|底对齐	|左对齐|
|StageAlign.BOTTOM_RIGHT	|底对齐	|右对齐|

使舞台左对齐顶对齐: `stage.align = StageAlign.TOP_LEFT`; 


## stage缩放属性: scaleMode
一个 StageScaleMode 类中指定要使用哪种缩放模式的值。 以下是有效值：

+ `StageScaleMode.EXACT_FIT` -- 整个 Flash 应用程序在指定区域中可见，且不发生扭曲，同时保持应用程序的原始高宽比。 应用程序的两侧可能会显示边框。
+ `StageScaleMode.SHOW_ALL` -- 整个 Flash 应用程序在指定区域中可见，但不尝试保持原始高宽比。 可能会发生扭曲。
+ `StageScaleMode.NO_BORDER` -- 整个 Flash 应用程序填满指定区域，不发生扭曲，但有可能进行一些裁切，同时保持应用程序的原始高宽比。
+ `StageScaleMode.NO_SCALE` -- 整个 Flash 应用程序的大小固定，因此，即使播放器窗口的大小更改，它也会保持不变。 如果播放器窗口比内容小，则可能进行一些裁切。


`ex. stage.scaleMode = StageScaleMode.NO_SCALE;`


## fullScreen事件
若要启用全屏模式，请将 `allowFullScreen` 参数添加到包含 SWF 文件的 HTML 页中的 object 和 embed 标签，同时将 allowFullScreen 设置为 "true"，如下例所示：

`<param name="allowFullScreen" value="true" />`

给swf增加一个全屏按钮: fullBt
代码如下：

```
fullBt.addEventListener(MouseEvent.CLICK,fullscreenshow);
function fullscreenshow(evt:MouseEvent):void {
  switch (stage.displayState) {
    case "normal" :
      stage.displayState = "fullScreen";
      break;
    case "fullScreen" :
    default :
     stage.displayState = "normal";
     break;
   }
}```


## resize事件

resize事件，可以用来制作自适就尺寸的swf，当swf的播放窗口size改变，触发该事件。