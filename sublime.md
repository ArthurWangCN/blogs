---
title: Almighty Sublime Text 3
date: 2017-02-20 22:39:32
tags: 工具
categories: 工具
---
在前端工程师的世界里，`Sublime Text` 一直是最受欢迎的文本编辑器之一，Sublime Text具有漂亮的用户界面和强大的功能，例如代码缩略图，Python的插件，代码段等。还可自定义键绑定，菜单和工具栏。Sublime Text 的主要功能包括：拼写检查，书签，完整的 Python API ， Goto 功能，即时项目切换，多选择，多窗口等等。Sublime Text 是一个跨平台的编辑器，同时支持Windows、Linux、Mac OS X等操作系统。

![sublime logo](http://blogpic.at15cm.com/sublime1.png)

下面就介绍一下Sublime Text安装过程以及插件的安装：

## 下载
去官网下载 [Sublime Text](http://www.sublimetext.com/)，点击`download`，官网会根据你的电脑系统自动为你选择下载的版本。

![download sublime](http://blogpic.at15cm.com/sublime2.png)

## 安装
在windows下下载的是.exe格式的可执行文件，双击打开按照提示的步骤一步一步安装,mac下同理。

![setup](http://blogpic.at15cm.com/sublime3.png)

## 注册
安装完成之后打开Sublime Text，找到 `help`，在下面有一栏是输入 `license` 的，我的是已经输入license的，所以help的选项发生变化了，附录中有注册码。

![register](http://blogpic.at15cm.com/sublime4.png)

## 插件
Sublime Text的强大之处在于它有丰富的插件，安装Sublime Text 3插件的方法：

**直接安装**

安装Sublime text 插件很方便，可以直接下载安装包解压缩到Packages目录（菜单->preferences->packages）。

**使用 `Package Control` 组件安装**

也可以安装package control组件，然后直接在线安装：

+ 按 *Ctrl+`* 调出 console（注：安装有QQ输入法的这个快捷键会有冲突的，输入法属性设置-输入法管理-取消热键切换至QQ拼音），或者点击 *view->show console*。
+ 粘贴以下代码到底部命令行并回车：

```
import urllib.request,os;
pf = 'Package
Control.sublime-package';
ipp = sublime.installed_packages_path();
urllib.request.install_opener( urllib.request.build_opener(
urllib.request.ProxyHandler())
); open(os.path.join(ipp,
pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace('
','
')).read())```


+ 重启Sublime Text 3。
+ 如果在Perferences->package settings中看到package control这一项，则安装成功。

顺便贴下Sublime Text2 的代码:
```
import urllib2,os;
pf='Package
Control.sublime-package';
ipp = sublime.installed_packages_path();
os.makedirs( ipp ) if not os.path.exists(ipp) else None;
urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler(
))); open(
os.path.join( ipp, pf), 'wb' ).write(
urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( '
','
' )).read()); print( 'Please
restart Sublime Text to finish installation')```

如果这种方法不能安装成功，可以手动安装。

**用Package Control安装插件的方法：**

+ 按下 `Ctrl+Shift+P` 调出命令面板
+ 输入 `install` 调出 `Install Package` 选项并回车，然后在列表中选中要安装的插件。

不爽的是，有的网络环境可能会不允许访问陌生的网络环境从而设置一道防火墙，而Sublime Text 2貌似无法设置代理，可能就获取不到安装包列表了。

***

好，方法介绍完了，下面是本文正题，一些有用的Sublime Text插件：

**GBK Encoding Support**
对应gb2312来说，Sublime Text 2 本身不支持的，我们可以通过Ctrl+Shift+P调出命令面板或Perferences->Package Control,输入install 调出 Install Package 选项并回车，在输入“GBK Encoding Support”选择开始安装，左下角状态栏有提示安装成功。这时打开gbk编码的文件就不会出现乱码了，如果有需要转成utf-8的可以在File-GBK to UTF8-选择Save with UTF8就偶看了。

**Zen Coding**
这个，不解释了，还不知道 `ZenCoding` 的同学强烈推荐去看一下：《Zen Coding: 一种快速编写HTML/CSS代码的方法》。

**emmet**
PS:Zen Coding for Sublime Text 2插件的开发者已经停止了在Github上共享了，现在只有通过Package Control来安装。

**jQuery Package for sublime Text**
如果你离不开jQuery的话，这个必备～～

**Sublime Prefixr**
Prefixr，CSS3 私有前缀自动补全插件，显然也很有用哇

**JS Format**
一个JS代码格式化插件。

**SublimeLinter**
一个支持lint语法的插件，可以高亮linter认为有错误的代码行，也支持高亮一些特别的注释，比如“TODO”，这样就可以被快速定位。（IntelliJ IDEA的TODO功能很赞，这个插件虽然比不上，但是也够用了吧）

**Placeholders**
故名思意，占位用，包括一些占位文字和HTML代码片段，实用。

**Sublime Alignment**
用于代码格式的自动对齐。传说最新版Sublime 已经集成。
 
**Clipboard History**
粘贴板历史记录，方便使用复制/剪切的内容。

**DetectSyntax**
这是一个代码检测插件。

**Nettuts Fetch**
如果你在用一些公用的或者开源的框架，比如 Normalize.css或者modernizr.js，但是，过了一段时间后，可能该开源库已经更新了，而你没有发现，这个时候可能已经不太适合你的项目了，那么你就要重新折腾一遍或者继续用陈旧的文件。Nettuts Fetch可以让你设置一些需要同步的文件列表，然后保存更新。

 
**JsMinifier**
该插件基于Google Closure compiler，自动压缩js文件。

**Sublime CodeIntel**
代码自动提示

**Bracket Highlighter**
类似于代码匹配，可以匹配括号，引号等符号内的范围。

**Hex to HSL**
自动转换颜色值，从16进制到HSL格式，快捷键 Ctrl+Shift+U

**GBK to UTF8**
将文件编码从GBK转黄成UTF8，快捷键Ctrl+Shift+C

**Git**
该插件基本上实现了git的所有功能。

###总结：
Sublime Text是一款很棒的文本编辑器，它体积小，功能强大，用户界面友好，受到很多程序员的青睐。但是在安装过程中并不是一帆风顺的，由于网络的原因会出现package control安装失败的情况，如果出现下面图片的提示，尝试手动安装，或者换换网络环境多尝试几次。BTW，如果经济条件允许请支持整版。

![error](http://blogpic.at15cm.com/sublime5.png)

附：sublime text 3 3114 注册码

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">—– BEGIN LICENSE —–
Michael Barnes
Single User License
EA7E-821385
8A353C41 872A0D5C DF9B2950 AFF6F667
C458EA6D 8EA3C286 98D1D650 131A97AB
AA919AEC EF20E143 B361B1E7 4C8B7F04
B085E65E 2F5F5360 8489D422 FB8FC1AA
93F6323C FD7F7544 3F39C318 D95E6480
FCCC7561 8A4A1741 68FA4223 ADCEDE07
200C25BE DBBC4855 C4CFB774 C5EC138C
0FEC1CEF D9DCECEC D3A5DAD1 01316C36
—— END LICENSE ——</div>
<br/>

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">—– BEGIN LICENSE —–
Free Communities Consultoria em Informática Ltda
Single User License
EA7E-801302
C154C122 4EFA4415 F1AAEBCC 315F3A7D
2580735A 7955AA57 850ABD88 72A1DDD8
8D2CE060 CF980C29 890D74F2 53131895
281E324E 98EA1FEF 7FF69A12 17CA7784
490862AF 833E133D FD22141D D8C89B94
4C10A4D2 24693D70 AE37C18F 72EF0BE5
1ED60704 651BC71F 16CA1B77 496A0B19
463EDFF9 6BEB1861 CA5BAD96 89D0118E
—— END LICENSE ——</div>
<br/>

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">—– BEGIN LICENSE —–
Nicolas Hennion
Single User License
EA7E-866075
8A01AA83 1D668D24 4484AEBC 3B04512C
827B0DE5 69E9B07A A39ACCC0 F95F5410
729D5639 4C37CECB B2522FB3 8D37FDC1
72899363 BBA441AC A5F47F08 6CD3B3FE
CEFB3783 B2E1BA96 71AAF7B4 AFB61B1D
0CC513E7 52FF2333 9F726D2C CDE53B4A
810C0D4F E1F419A3 CDA0832B 8440565A
35BF00F6 4CA9F869 ED10E245 469C233E
—— END LICENSE ——</div>
<br/>

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">—– BEGIN LICENSE —–
Anthony Sansone
Single User License
EA7E-878563
28B9A648 42B99D8A F2E3E9E0 16DE076E
E218B3DC F3606379 C33C1526 E8B58964
B2CB3F63 BDF901BE D31424D2 082891B5
F7058694 55FA46D8 EFC11878 0868F093
B17CAFE7 63A78881 86B78E38 0F146238
BAE22DBB D4EC71A1 0EC2E701 C7F9C648
5CF29CA3 1CB14285 19A46991 E9A98676
14FD4777 2D8A0AB6 A444EE0D CA009B54
—— END LICENSE ——</div>
<br/>

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">—– BEGIN LICENSE —–
Alexey Plutalov
Single User License
EA7E-860776
3DC19CC1 134CDF23 504DC871 2DE5CE55
585DC8A6 253BB0D9 637C87A2 D8D0BA85
AAE574AD BA7D6DA9 2B9773F2 324C5DEF
17830A4E FBCF9D1D 182406E9 F883EA87
E585BBA1 2538C270 E2E857C2 194283CA
7234FF9E D0392F93 1D16E021 F1914917
63909E12 203C0169 3F08FFC8 86D06EA8
73DDAEF0 AC559F30 A6A67947 B60104C6
—— END LICENSE ——</div>
<br/>

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">—– BEGIN LICENSE —–
Peter Halliday
Single User License
EA7E-855988
3997BFF0 2856413A 7A555954 67069B78
06D8CE12 63EAF079 AD039757 79E16D13
C555AD90 465CBE53 10F6DFC4 D3A3C611
411106F8 0CFEB15F 0A7BB891 111F5ED2
C6AA8429 77913528 FA6291A9 B88D4550
F1D6AB13 BF9153BC 91B4DFFE D296CFE0
C1D8EB22 13D5F14E 75A699EC 49EDDC23
D89D0F9B D240B10A A3712467 09DE7870
—— END LICENSE ——</div>
<br/>

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">—– BEGIN LICENSE —–
Fred Zirdung
Single User License
EA7E-844672
6089C0EC 22936E1A 1EADEBE2 B8654BBA
5C98FFA6 C0FD1599 0364779B 071C74FB
EEFE9EAB 92B3D867 CD1B32FE D190269F
6FC08F8F 8D24191D 32828465 942CE58E
AECE5307 08B62229 D788560A 6E0AAC4B
48A2D9EE 24FD8CAA 07BEBDF2 28EA86D4
CCB96084 6C34CAD2 E8A04F39 3B5A3CBC
3B668BB7 C94D0B4B 847D6D7F 4BC07375
—— END LICENSE ——</div>
<br/>

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">—– BEGIN LICENSE —–
Wixel
Single User License
EA7E-848235
103D2969 8700C7ED 8173CF61 537000C0
EB3C7ECB 5E750F17 6B42B67C A190090B
7669164F C6F371A8 5A1D88D5 BDD0DA70
C065892B 7CC1BB2B 1C8B8C7C F08E7789
7C2A5241 35F86328 4C8F70D9 C023D7C2
11245C36 59A730DB 72BDB9A7 D5B20304
90E90E72 9F08CA25 73F49C20 179D938E
5BC8BEDA 13457A69 39E6265F 233767F9
—— END LICENSE ——</div>
<br/>

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">—– BEGIN LICENSE —–
Daniel Russel
Single User License
EA7E-917420
9327EC62 44020C2A 45172A68 12FE13F1
1D22245B 680892EE F551F8EB C183D032
8B4EDB4B 479CB7E4 07E42EDD A780021D
56BADF42 AC05238B 023B47B1 EBA1B7DE
6DF9A383 159F32AE 04EBE100 1278B1D2
52E81B60 C68AA2E8 F84A20BE FE7990EB
5D44E4B6 16369263 1DDAACBC 280FF19E
86CF4319 0B8615A8 4FF0512E B123B8EC
—— END LICENSE ——</div>
<br/>

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">
—– BEGIN LICENSE —–
Peter Eriksson
Single User License
EA7E-890068
8E107C71 3100D6FC 2AC805BF 9E627C77
72E710D7 43392469 D06A2F5B F9304FBD
F5AB4DB2 7A95F172 FE68E300 42745819
E94AB2DF C1893094 ECABADC8 71FEE764
20224821 3EABF931 745AF882 87AD0A4B
33C6E377 0210D712 CD2B1178 82601542
C7FD8098 F45D2824 BC7DFB38 F1EBD38A
D7A3AFE0 96F938EA 2D90BD72 9E34CDF0
—— END LICENSE ——</div>
<br/>

<div style="color:#000;background-color:#EDEDED;border-radius:6px;padding:30px 20px">—– BEGIN LICENSE —–
Ryan Clark
Single User License
EA7E-812479
2158A7DE B690A7A3 8EC04710 006A5EEB
34E77CA3 9C82C81F 0DB6371B 79704E6F
93F36655 B031503A 03257CCC 01B20F60
D304FA8D B1B4F0AF 8A76C7BA 0FA94D55
56D46BCE 5237A341 CD837F30 4D60772D
349B1179 A996F826 90CDB73C 24D41245
FD032C30 AD5E7241 4EAA66ED 167D91FB
55896B16 EA125C81 F550AF6B A6820916
—— END LICENSE ——</div>


#### 参考文献
+ [Sublime Text 3 3114 注册码](https://fatesinger.com/78252)
