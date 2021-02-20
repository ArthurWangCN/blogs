---
title: DPI DIP 分辨率 屏幕尺寸 px density 关系以及换算
date: 2017-05-03 13:39:11
tags: [分辨率, 移动端]
categories: 前端
---
## 基本概念
+ dip : Device independent pixels（设备独立像素）；
+ dp  : 即 `dip`；
+ px : 像素；
+ dpi : dots per inch，直接来说就是一英寸多少个像素点。常见取值 120，160，240。我一般称作像素密度，简称密度；
+ density : 直接翻译的话貌似叫 `密度`。常见取值 `1.5`，`1.0`。和标准dpi的比例（160px/inc）
+ 分辨率 : 横纵2个方向的像素点的数量，常见取值 `480X800`，`320X480`；
+ 屏幕尺寸 : 屏幕对角线的长度。电脑电视同理；
+ 屏幕比例的问题 : 因为只确定了对角线长，两边长度还不一定。所以有了4：3、16：9这种，这样就可以算出屏幕边长了。


## 应用
在 `android` 里面，获取一个窗口的 `metrics`，里面有这么几个值

```
metrics.density; 
metrics.densityDpi;
```

densityDpi : 就是我们常说的 `dpi`。
density : 其实是 `DPI/(160像素/英寸)` 后得到的值。

从上面就看得出了，DPI 本身的单位也是 `像素/英寸`，所以 density 其实是没单位的，他就是一个比例值。

而 dpi 的单位是 `像素/英寸`，比较符合物理上面的密度定义，密度不都是单位度量的值么，所以我更喜欢把 dpi 叫像素密度，简称密度，density还是就叫density。

 
## 各单位间转换
### 计算dpi　
比如一个机器，屏幕4寸，分辨率480X800，他的dpi能算么。

因为不知道边长，肯定不能分开算，4是对角线长度，那直接用勾股定理算对角线像素，除以4，算出来大概是 dpi = 233 像素/英寸。

那么density就是 （233 px/inch）/（160 px/inch）= 1.46 左右

顺带说下，android 默认的只有 3 个dpi，low、medium和high，对应 120、160、240，如果没有特别设置，所有的dpi都会被算成这3个，具体可以参考 [Android中density如何设置](http://android.tgbus.com/Android/tutorial/201103/347176.shtml)

其中的default就是160。


### 计算 dp 与 px
我们写布局的时候，肯定还是要知道1个dp到底有多少px的。

换算公式如下： `dp = （DPI/（160像素/英寸））px = density px`

注意，这里都是带单位的。px是单位，dp有单位，density没单位。

为了方便，假设dpi是 240 像素/英寸 ， 那么density就是1.5
那么就是 dp=1.5px ，注意这是带了单位的，也就是 `设备独立像素 = density 像素`

那么转换为数值计算的话，应该是下面这个式子

```
PX = density * DP
```

也就是
像素值 = density * 设备独立像素值 ，请注意这里有个值字。


### 为什么标准dpi = 160
Android Design 里把主流设备的 dpi 归成了四个档次，120 dpi、160 dpi、240 dpi、320 dpi

实际开发当中，我们经常需要对这几个尺寸进行相互转换（比如先在某个分辨率下完成设计，然后缩放到其他尺寸微调后输出），一般按照 dpi 之间的比例即 `2:1.5:1:0.75` 来给界面中的元素来进行尺寸定义。

也就是说如果以 160 dpi 作为基准的话，只要尺寸的 DP 是 4 的公倍数，XHDPI 下乘以 2，HDPI 下乘以 1.5，LDPI 下乘以 0.75 即可满足所有尺寸下都是整数 pixel 。

但假设以 240 dpi 作为标准，那需要 DP 是 3 的公倍数，XHDPI 下乘以 1.333，MDPI 下乘以 0.666 ，LDPI 下除以 2

而以 LDPI 和 XHDPI 为基准就更复杂了，所以选择 160 dpi

>注：这个在Google的官方文档中有给出了解释，因为第一款Android设备（HTC的T-Mobile G1）是属于160dpi的。

 
## 示例分析
### 屏幕尺寸(screen size)
就是我们平常讲的手机屏幕大小，是屏幕的对角线长度，一般讲的大小单位都是英寸。

比如 iPhone5S 的屏幕尺寸是4英寸。Samsung Note3 是5.7英寸。

![小米5](http://blogpic.at15cm.com/MI5.png)

### 像素(pixel)
想像把屏幕放大再放大，看到的那一个个小点或者小方块就是像素了。

### 分辨率(Resolution)
是指屏幕上垂直方向和水平方向上的像素个数。

比如 iPhone5S 的分辨率是 `1136*640`；Samsung Note3 的分辨率是`1920*1080`；

### dpi
是dot per inch的缩写，就是每英寸的像素数，也叫做屏幕密度。这个值越大，屏幕就越清晰。

iPhone5S的dpi是326； Samsung Note3 的dpi是386

### dip
是Device independent pixel的缩写，指的是抽象意义上的像素。跟设备的屏幕密度有关系。

它是Android里的一个单位，dip和dp是一样的。

Google的官方说明是这样的：

Density-independent pixel (dp)
A virtual pixel unit that you should use when defining UI layout, to express layout dimensions or position in a density-independent way.
The density-independent pixel is equivalent to one physical pixel on a 160 dpi screen, which is the baseline density assumed by the system for a "medium" density screen. At runtime, the system transparently handles any scaling of the dp units, as necessary, based on the actual density of the screen in use. The conversion of dp units to screen pixels is simple: px = dp * (dpi / 160). For example, on a 240 dpi screen, 1 dp equals 1.5 physical pixels. You should always use dp units when defining your application's UI, to ensure proper display of your UI on screens with different densities.


就是说在160dpi的屏幕上，1dip=1px。跟屏幕密度有关，如果屏幕密度大，1dip代表的px就多，比如在320dpi的屏幕上，1dip=2px。

为什么我们在布局的时候最好要用dip，不要用px？
是因为这个世界上存在着很多不同屏幕密度的手机，屏幕密度是什么？就是dpi，就是单位长度里的像素数量。

想象一下，如果这些手机的尺寸一样，屏幕密度相差很大，那么是不是说一个手机水平方向上像素很少，另一个手机水平方向上像素很多？那我们画同样pix数量的时候，它显示的长度不就会不一样了？

比如下面图中的两个手机，同时设置2px长度的Button，在屏幕密度较高的手机里就会显示的比较小。而同时设置的2dip长度的Button，在两个手机上显示的大小是一样的。

![](http://images.cnitblog.com/i/66979/201407/141715402871566.png)

所以如果你在App布局中都用的px作为单位，那么你的App跑在各个设备上就会出现奇奇怪怪的现象了。


## 参考文献
+ [dpi 、 dip 、分辨率、屏幕尺寸、px、density 关系以及换算](http://www.cnblogs.com/yaozhongxiao/archive/2014/07/14/3842908.html)
