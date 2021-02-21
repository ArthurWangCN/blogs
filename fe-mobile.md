---
title: 移动端知识总结
date: 2020-05-26 14:45:33
tags: 前端
categories: 前端
---



## meta基础知识

**1、H5页面窗口自动调整到设备宽度，并禁止用户缩放页面**

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
```



**2、忽略将页面中的数字识别为电话号码**

```html
<meta name="format-detection" content="telephone=no" />
```



**3、忽略[Android](http://caibaojian/t/android)平台中对邮箱地址的识别**

```html
<meta name="format-detection" content="email=no" />
```



**4、当网站添加到主屏幕快速启动方式，可隐藏地址栏，仅针对ios的safari**

```html
<meta name="apple-mobile-web-app-capable" content="yes" />
<!-- ios 7.0版本以后，safari上已看不到效果 -->
```



**5、将网站添加到主屏幕快速启动方式，仅针对ios的safari顶端状态条的样式**

```html
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<!-- 可选default、black、black-traslucent -->
```



## viewport

**viewport模板**

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" name="viewport" />
    <meta content="yes" name="apple-mobile-web-app-capable" />
    <meta content="black" name="apple-mobile-web-app-status-bar-style" />
    <meta name="format-detection" content="telephone=no" />
    <meta name="format-detection" content="email=no" />
    <title>标题</title>
    <link rel="stylesheet" href="index.css" />
  </head>
  <body>
    这里开始内容
  </body>
</html>
```



## 常见问题

**1、移动端如何定义字体**

中文字体使用系统默认即可，英文用Helvetica

```css
body{ font-family: Helvetiva; }
```



**2、移动端字体单位font-size选择px还是rem**

+ 对于只需要适配少部分手机设备，且分辨率对页面影响不大的，使用px即可。

+ 对于需要适配各种移动设备，使用rem，例如只需要适配iphone和iPad等分辨率差别比较挺大的设备。

rem配置参考：

```css
html{ font-size: 10px; }
@media screen and (min-width: 321px) and (max-width: 375px){ font-size: 11px; }
@media screen and (min-width: 376px) and (max-width: 414px){ font-size: 12px; }
@media screen and (min-width: 415px) and (max-width: 639px){ font-size: 15px; }
@media screen and (min-width: 640px) and (max-width: 719px){ font-size: 20px; }
@media screen and (min-width: 720px) and (max-width: 749px){ font-size: 22.5px; }
@media screen and (min-width: 750px) and (max-width: 799px){ font-size: 23.5px; }
@media screen and (min-width: 8000px){ font-size: 25px; }
```



**3、移动端touch事件（区分webkit和winphone）**

 当用户手指放在移动设备在屏幕上滑动会触发的 touch 事件。

以下支持webkit：

+ touchstart —— 当手指触碰屏幕时候发生。不管当前有多少只手指；
+ touchmove —— 当手指在屏幕上滑动时连续触发。通常我们再滑屏页面，会调用event的 preventDefault() 可以阻止默认情况的发生：阻止页面滚动；
+ touchend —— 当手指离开屏幕时触发；
+ touchcancel —— 系统停止跟踪触摸时候会触发。例如在触摸过程中突然页面alert()一个提示框，此时会触发该事件，这个事件比较少用；



TouchEvent：

+ touches：屏幕上所有手指的信息；
+ targetTouches：手指在目标区域的手指信息；
+ changedTouches：最近一次触发该事件的手指信息；
+ touchend时，touches与targetTouches信息会被删除，changedTouches保存的最后一次的信息，最好用于计算手指信息。

参数信息(changedTouches[0])

+ clientX、clientY在显示区的坐标;
+ target：当前元素



以下支持winphone 8

+ MSPointerDown——当手指触碰屏幕时候发生。不管当前有多少只手指;
+ MSPointerMove——当手指在屏幕上滑动时连续触发。通常我们再滑屏页面，会调用css的html{-ms-touch-action: none;}可以阻止默认情况的发生：阻止页面滚动;
+ MSPointerUp——当手指离开屏幕时触发;



**4、移动端300ms延迟**

移动设备上的web网页是有300ms延迟的，往往会造成按钮点击延迟甚至是点击失效。

> 2007年苹果发布首款iphone上IOS系统搭载的safari为了将适用于PC端上大屏幕的网页能比较好的展示在手机端上，使用了双击缩放(double tap to zoom)的方案，比如你在手机上用浏览器打开一个PC上的网页，你可能在看到页面内容虽然可以撑满整个屏幕，但是字体、图片都很小看不清，此时可以快速双击屏幕上的某一部分，你就能看清该部分放大后的内容，再次双击后能回到原始状态。
>
> 双击缩放是指用手指在屏幕上快速点击两次，iOS 自带的 Safari 浏览器会将网页缩放至原始比例。

原因就出在浏览器需要如何判断快速点击上，当用户在屏幕上单击某一个元素时候，例如跳转链接 `<a href="#"></a>`，此处浏览器会先捕获该次单击，但浏览器不能决定用户是单纯要点击链接还是要双击该部分区域进行缩放操作，所以，捕获第一次单击后，浏览器会先Hold一段时间t，如果在t时间区间里用户未进行下一次点击，则浏览器会做单击跳转链接的处理，如果t时间里用户进行了第二次单击操作，则浏览器会禁止跳转，转而进行对该部分区域页面的缩放操作。那么这个时间区间t有多少呢？在IOS safari下，大概为300毫秒。这就是延迟的由来。造成的后果用户纯粹单击页面，页面需要过一段时间才响应，给用户慢体验感觉，对于web开发者来说是，页面js捕获click事件的回调函数处理，需要300ms后才生效，也就间接导致影响其他业务逻辑的处理。



解决方案：

+ fastclick可以解决在手机上点击事件的300ms延迟
+ zepto的touch模块，tap事件也是为了解决在click的延迟问题



触摸事件的响应顺序：

+ ontouchstart
+ ontouchmove
+ ontouchend
+ onclick



解决300ms延迟的问题，也可以通过绑定 `ontouchstart` 事件，加快对事件的响应



**5、什么是Retina 显示屏，带来了什么问题**

 retina：一种具备超高像素密度的液晶屏，同样大小的屏幕上显示的像素点由1个变为多个，如在同样带下的屏幕上，苹果设备的retina显示屏中，像素点1个变为4个。

在高清显示屏中的位图被放大，图片会变得模糊，因此移动端的视觉稿通常会设计为传统PC的2倍。

前端应对方案：

设计稿切出来的图片长宽保证为偶数，并使用backgroud-size把图片缩小为原来的1/2。

```css
/* 图片宽高为200px*200px */
.css{
  width: 100px;
  height: 100px;
  background-size: 100px 100px;
}
/* 其他元素取值为原来的1/2 ，如40px的字体，写为20px */
.css{
  font-size: 20px;
}
```



**6、元素被触摸产生的问题**

ios系统中元素被触摸时产生的半透明灰色遮罩怎么去掉？

ios用户点击一个链接，会出现一个半透明灰色遮罩, 如果想要禁用，可设置 `-webkit-tap-highlight-color的alpha` 值为 0，也就是属性值的最后一位设置为0就可以去除半透明灰色遮罩

```css
a, button, input, textarea {-webkit-tap-highlight-color: rgba(0, 0, 0, 0);}
```



部分android系统中元素被点击时产生的边框怎么去掉?

android用户点击一个链接，会出现一个边框或者半透明灰色遮罩, 不同生产商定义出来额效果不一样，可设置 `-webkit-tap-highlight-color` 的 alpha 值为 0 去除部分机器自带的效果。

```css
a, button, input, textarea {
	-webkit-tap-highlight-color: rgba(0, 0, 0, 0);
	-webkit-user-modify: read-write-plaintext-only;
}
```

-webkit-user-modify有个副作用，就是输入法不再能够输入多个字符。另外，有些机型去除不了，如小米2。

对于按钮类还有个办法，不使用a或者input标签，直接用div标签。



winphone系统a、input标签被点击时产生的半透明灰色背景怎么去掉？

```html
<meta name="msapplication-tap-highlight" content="no" />
```



**7、表单相关**

webkit表单元素的默认外观怎么重置：

```css
.css{-webkit-appearance: none;}
```



webkit表单输入框placeholder的颜色值能改变么:

```css
input::-webkit-input-placeholder{color: #AAAAAA;}
input:focus::-webkit-input-placeholder{color: #EEEEEE;}
```



IE10（winphone8）表单元素默认外观如何重置？

禁用select默认下拉箭头 —— ::-ms-expand 适用于表单选择控件下拉箭头的修改，有多个属性值，设置它隐藏 (display:none) 并使用背景图片来修饰可得到我们想要的效果。

```css
select::-ms-expand{ display: none; }
```



禁用 radio 和 checkbox 默认样式 —— ::-ms-check 适用于表单复选框或单选按钮默认图标的修改，同样有多个属性值，设置它隐藏 (display:none) 并使用背景图片来修饰可得到我们想要的效果。

```css
input[type=radio]::-ms-check,
input[type=checkbox]::-ms-check
{ display: none; }
```



禁用PC端表单输入框默认清除按钮 —— 当表单文本输入框输入内容后会显示文本清除按钮，::-ms-clear 适用于该清除按钮的修改，同样设置使它隐藏 (display:none) 并使用背景图片来修饰可得到我们想要的效果。

```css
input[type=text]::-ms-clear,
input[type=tel]::-ms-clear,
input[type=number]::-ms-clear
{ display: none; }
```



禁止ios 长按时不触发系统的菜单，禁止ios&android长按时下载图片:

```css
.css{ _webkit-touch-callout: none; }
```



禁止ios和android用户选中文字:

```css
.css{ -webkit-user-select: none; }
```



**8、打电话发短信写邮件**

打电话：

```html
<a href="tel:xxxx-xxxxxxx">打电话给xxxxxx</a>
```

发短信(winphone无效)：

```html
<a href="sms:xxxxx">发短信给xxxxx</a>
```

发邮件：

```html
<a href="mailto:abc@163.com">发邮件给abc</a>
```



**9、屏幕旋转的事件和样式**

 window.orientation，取值：正负90表示横屏模式、0和180表现为竖屏模式；

```javascript
window.onorientationchange = function() {
  switch(window.orientation) {
    case -90:
    case 90:
      console.log("横屏" + window.orientation);
    case 0:
    case 180:
      console.log("竖屏" + window.orientation);
    	break;
  }
}
```



样式：

```css
/* 竖屏 */
@media all and (orientation: portrait) { xxx }
/* 横屏 */
@media all and (orientation: landscape) { xxx }
```





**10、摇一摇功能**

HTML5 deviceMotion：封装了运动传感器数据的事件，可以获取手机运动状态下的运动加速度等数据。



**11、手机拍照和上传图片**

```html
选择照片
<input type="file"
       id="avatar" name="avatar"
       accept="image/png, image/jpeg">
```



**12、 微信浏览器用户调整字体大小后页面出现问题**

原因：

+ android侧是复写了layoutinflater 对textview做了统一处理；
+  ios侧是修改了body.style.webkitTextSizeAdjust值；

解决：

android使用如下代码，只在微信浏览器下有效：

```javascript
/**
    * 页面加入这段代码可使Android机器页面不再受到用户字体缩放强制改变大小
    * 但是会有一个1秒左右的延迟，期间可以考虑通过loading展示
    * 仅供参考
    */
(function () {
    if (typeof (WeixinJSBridge) == "undefined") {
        document.addEventListener("WeixinJSBridgeReady", function (e) {
            setTimeout(function () {
                WeixinJSBridge.invoke('setFontSizeCallback', {
                    "fontSize": 0
                }, function (res) {
                    alert(JSON.stringify(res));
                });
            }, 0);
        });
    } else {
        setTimeout(function () {
            WeixinJSBridge.invoke('setFontSizeCallback', {
                "fontSize": 0
            }, function (res) {
                alert(JSON.stringify(res));
            });
        }, 0);
    }
})();
```

ios使用-webkit-text-size-adjust禁止调整字体大小:

```css
body{-webkit-text-size-adjust: 100% !important;}
```

最好的解决方案：

整个页面用rem或百分比布局



**13、取消input在ios下，输入的时候英文首字母的默认大写**

```html
<input autocapitalize="off" autocorrect="off" />
```



**14、android 上去掉语音输入按钮**

```css
input::-webkit-input-speech-button{display:none;}
```





## 常用的移动端框架

### zepto.js



### iscroll.js

解决页面不支持弹性滚动，不支持fixed引起的问题。

实现下拉刷新，滑屏，缩放等功能。



### underscore.js

该库提供了一整套函数式编程的实用功能，但是没有扩展任何



### 滑屏框架

适合上下滑屏、左右滑屏等滑屏切换页面的效果。

+ slip.js
+ iSlider.js
+ fullpage.js



### FastClick

消除在移动浏览器上触发click事件与一个物理Tap(敲击)之间的300延迟





