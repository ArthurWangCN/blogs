---
title: 聊聊为什么不喜欢苹果机（伪技术博）
date: 2018-06-05 15:57:23
tags: [游戏开发, WeTest, 杂谈]
categories: 杂谈
---
## The reason why I'm not interested in iPhone

**我不喜欢苹果机！打小就不喜欢！**

最近接手了一个答题类游戏，功能完善后要对接小游戏，最后的验收阶段要求有性能测试，所以就找来了一大堆的手机来测试，安卓机三档，苹果机三档。

第一天安卓机测试还算顺利，除了红米note2间歇性断开连接（很可能是硬件的问题），h5argusqt_wx完美的完成了他的任务。出图，截图，分析数据，就等着写报告了。

第二天苹果机拿来了，一台iPhone 5C，一台iPhone 6S Plus。品管要求用WeTest测试性能，那就打开 [官网](http://wetest.qq.com/cloud/help/client) 瞅一眼吧。很棒棒，需要用Cydia来下载WeTest，可是他喵的Cydia是什么鬼？

>iPhone、iPod touch、iPad等设备上的一种破解软件，类似苹果在线软件商店iTunes Store 的软件平台的客户端，在越狱的过程中被装入到系统中的，其中多数为iPhone、iPod Touch、ipad的第三方软件和补丁，主要都是弥补系统不足用。是由Jay Freeman（Saurik）领导，Okori Group以及UCSB大学合作开发。

**你直接说盗版AppStore不得了，整那么多没用的！**

走到这一步后我的安卓思维告诉我去哪里下载一个Cydia安装包，但是这是苹果，不灵。你在网上搜个遍，发现越狱是装Cydia最正确的道路，可是之前总是听别人说越狱，他喵的越狱又是什么鬼？

>越狱如果针对部分苹果设备来讲，是针对苹果操作系统（IOS系统）限制用户存储读写权限的破解操作。经过越狱的iPhone拥有对系统底层的读写权限，能够让苹果（iPhone）手机免费使用破解后的App Store软件的程序（相当于盗版）。

明白啥意思了吧，就是苹果不让你用盗版的东西，你说声“妖怪吧，劳资就是要用！”

问一下上面的人说可以越，越就越吧，这里也不乱扯了，越狱推荐 [PP助手](https://pro.25pp.com/) 和 [爱思助手](https://www.i4.cn/)。别问我为什么推荐这俩，因为我只搜了这俩。

iphone5C成功越狱，成功的打开了Cydia，按照教程添加源 http://f.wetest.qq.com/wtios/ ，进入新添加的源，名称为"WeTest"，找到WeTest助手，安装，完成。然后开始测试，出报告，上传，数据分析，完美！

我以为iphone6s plus也会这么顺利，但是我还是太naive。其实6s plus也基本上快要成功了，开始测试进小游戏的时候卡了一下（这里就不过多吐槽公司的网了，真是慢！阿财，别用网！咳咳，串场了，是隔壁douyu688剧本，321~收）。然后退游戏，退微信，打开WeTest，闪退，无法卸载！打开Cydia，闪退，无法卸载！打开PP助手，闪退，无法卸载！

**真是妖怪多！**

几小时后...

算了，重新越狱试试吧，打开爱思助手，越狱，一切又都好了！

MMP，此刻只想赶紧测完还手机，苹果机一秒都不想多看！

谨以此篇文章致敬所有测试人员，你们辛苦了！

！！！

<br/><br/><br/><br/><br/><br/>

桥豆麻袋！还没有结束！接下来说一下测试标准：

## 安卓测试标准
### TDR标准- Android平台机型定义
+ 3档机型： 
    
    配置 >=（CPU-四核 1.2GHZ，RAM-2G）
    
    测试机型：小米 note 
    
+ 2档机型： 
    
    配置 >=（CPU-四核 1.8GHZ，RAM-4G）
    
    测试机型：华为MATE8 
    
+ 1档机型： 
   
    配置 >=（CPU-八核 2.4GHZ，RAM-4G） 
    
    测试机型: 小米 mix2s
   
### TDR客户端性能标准简要
+ 3档机型

    内存： 最高PSS<=350MB(否则影响2900w，28%用户体验)
    
    帧率： 核心玩法中，最小FPS>=18
    
    CPU： CPU占用(90%)小于60%
    
    流量： 单局消耗流量不超过5M

+ 2档机型

    内存： 最高PSS<=450MB(否则影响3800w，42%用户体验)
    
    帧率： 核心玩法中，最小FPS>=25
    
    CPU： CPU占用(90%)小于60%
    
    流量： 单局消耗流量不超过5M

+ 1档机型

    内存： 最高PSS<=550MB(否则影响1700w，27%用户体验)
    
    帧率： 核心玩法中，最小FPS>=25
    
    CPU： CPU占用(90%)小于60%
    
    流量： 单局消耗流量不超过5M
    
## 苹果测试标准
### TDR标准- iPhone平台机型定义
+ 1档机型：

    配置 >=（CPU-苹果A10 Fusion四核处理器）

    建议测试机型：iPhone7
    
+ 2档机型：

    配置 <（CPU-苹果A9处理器）

    建议测试机型：iPhone6

+ 3档机型：

    配置 ≤（CPU-苹果A7处理器）
  
    建议测试机型：iPhone5S

### TDR客户端性能标准简要
+ 1档机型

    内存： 最高PSS<=500MB(否则影响1700w，28%用户体验)
    
    帧率： 核心玩法中，最小FPS>=25
    
    CPU： CPU占用(90%)小于60%
    
    流量： 单局消耗流量不超过5M

+ 2档机型

    内存： 最高PSS<=450MB(否则影响3800w，42%用户体验)
    
    帧率： 核心玩法中，最小FPS>=25
    
    CPU： CPU占用(90%)小于60%
    
    流量： 单局消耗流量不超过5M

+ 1档机型

    内存： 最高PSS<=360MB(否则影响2900w，27%用户体验)
    
    帧率： 核心玩法中，最小FPS>=18
    
    CPU： CPU占用(90%)小于60%
    
    流量： 单局消耗流量不超过5M
    
