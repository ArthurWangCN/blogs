---
title: 安卓客户端WeTest救命指南
date: 2018-06-12 15:41:13
tags: [WeTest, 游戏开发]
categories: Android
---
之前给手下的一个小游戏做性能测试，一档手机(vivo X20A)没有root所以没有拿到FPS数据，报告需要FPS，所以又把测试机搞过来测了一遍，故事从这里开始了...

原本以为是很简单的一件事，手机借到连上电脑root后测一下不得了，多大点事儿。然后就是重复下root软件，什么root精灵、360一键root、root大师、Kingroot统统的不行有木有，很强很螺旋！

为什么不行？直接问度娘vivo X20	A的root方法吧，结果...没有最绝望，只有更绝望：

![vivo x20a](http://blogpic.at15cm.com/vivox20a-1.jpg)

![vivo x20a](http://blogpic.at15cm.com/vivox20a-2.jpg)

![vivo x20a](http://blogpic.at15cm.com/vivox20a-3.jpg)

![vivo x20a](http://blogpic.at15cm.com/vivox20a-4.jpg)

惊不惊喜！意不意外！刺不刺激！

![daxian](http://blogpic.at15cm.com/daxian-chuinixiongkou.jpg)

评论区还有问候vivo家长的评论就不贴出来了。

做个弊，换台机器测吧，找了个小米MIX，开始root，root失败，完美！

![daxian](http://blogpic.at15cm.com/daxian-qidaofapang.jpg)

作为小米元老级用户，我竟然忘了去看手机系统是稳定版还是开发版，当然系统好不让我失望的写着稳定版！

想到之前测 [苹果机](http://legendaryarthur.cn/2018/06/05/why-hate-iphone/) 的时候用过 WeTest，正好这台测试机上也有，试试吧。

打开 [官网](http://wetest.qq.com/cloud/help/client)，很棒，再次给了我惊喜：

![wetest](http://blogpic.at15cm.com/wetest-android.jpg)

是的，没有root也是不行。

但是我是那种轻易放弃的人吗，经过我一番搜索，发现 WeTest 是有免 root 工具的。[[链接](http://wetest.qq.com/help/documentation/10303.html)]


## 免root测试流程

![免root测试流程](http://f.wetest.qq.com/gqop/10000/20000/GuideImage_b6c30e7479ee2f911f4d4f2dbe477505.png)


## 使用方法

### 下载测试工具
手机下载 WeTest，电脑下载 [免root工具](http://cdn.wetest.qq.com/com/c/cube-pc-64bit.zip)，没什么好说的。

### 测试手机连接PC
将手机通过USB连接电脑，此步骤需要 确保手机开启USB调试模式，USB与电脑连接正常。

### 手机Wetest助手
打开Wetest助手后，点击选择响应测试，并拉起待测试游戏，显示Wetest悬浮窗，测试成功开始。

![王者荣耀](http://f.wetest.qq.com/gqop/10000/20000/GuideImage_38818f95d2cd8e02cd6ef7a23fb24923.jpg)

如果弹出“后台程序未启动”，则需要重新将手机连接电脑，使用免ROOT工具进行连接并成功开启服务后，再次使用Wetest助手进行测试。


还有可能遇到一些其他稀奇古怪的情况，如果你比我还要怀疑人生，那么给你个[链接](http://wetest.qq.com/help/documentation/10303.html)，解决 `弹窗提示：未发现设备连接` 、 `弹窗提示：ADB server冲突`。

