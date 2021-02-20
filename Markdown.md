---
title: Markdown笔记
tags: Markdown
categories: 工具
---
>Markdown 是一种轻量级的「标记语言」，它的优点很多，目前也被越来越多的写作爱好者，撰稿者广泛使用。看到这里请不要被「标记」、「语言」所迷惑，Markdown 的语法十分简单。常用的标记符号也不超过十个，这种相对于更为复杂的 HTML 标记语言来说，Markdown 可谓是十分轻量的，学习成本也不需要太多，且一旦熟悉这种语法规则，会有一劳永逸的效果。

***

## 一、工具
+ 在 Mac OS X 上，建议你用 [Mou](http://25.io/mou/ "title") 这款免费且十分好用的 Markdown 编辑器，它支持实时预览，既左边是你编辑 Markdown 语言，右边会实时的生成预览效果。    

+ Windows 下的 Markdown 有两款还算不错，一款叫做 [MarkdownPad](http://markdownpad.com/ "title") ，另一款叫做 MarkPad。

***

## 二、语法

### 1.标题
<div style="color:#333;">Markdown支持两种标题的语法，Setext和atx形式。</div>

atx：由1~6个#（空格）和标题字组成，分别对应h1~h6
  
<div style="color:#fff;background-color:#000;border-radius:4px;padding:10px 20px;"><h1 style="color:#fff">一号标题(#)</h1><h2 style="color:#fff">二号标题(##)</h2><h3 style="color:#fff">三号标题(###)</h3><h4 style="color:#fff">四号标题(####)</h4><h5 style="color:#fff">五号标题(#####)</h5><h6 style="color:#fff">六号标题(######)</h6></div>

Setext：由标题字和下划线=或-组成，任何数量的=或-都可以，但为了避免出错，建议输入=或-的数量不少于3个

### 2.段落 
段落文本前要有一行或多行空白行  

### 3.粗体、斜体 
由*或_和文本组成，文本可以跨行但不可空行，文字之间可以有空格，单是斜体，双是粗体  

		*斜体*
		**粗体**

  效果：*斜体* &nbsp;&nbsp;**粗体**  

### 4.分割线
由至少三个`*`或`_`或`-`之一组成，单独一行可以有空格  

		***  	
效果：  
***
  
### 5.代码
单行内的代码使用反引号( \` \` )将代码包裹   
 
		这是`单行代码`
效果：这是`单行代码`    

大段的代码块要在每行前加4个`空格`或一个`tab`，而且有一些 HTML 区块元素――比如`\<div>`、`\<table>`、`\<pre>`、`\<p>` 等标签，必须在前后加上空行与其它内容区隔开，还要求它们的开始标签与结尾标签不能用制表符或空格来缩进  

### 6.使用HTML语言
可以直接使用HTML语言，但前后都要有空行  

	<div style="color:red">可以直接使用HTML语言，但前后都要有空行</div>  

<div style="color:red">可以直接使用HTML语言，但前后都要有空行</div>  
  
### 7.注释
Markdown中的注释与HTML的注释一样  

	<!-- Markdown中的注释与HTML的注释一样 -->  

### 8.转译
部分字符在Markdown中会自动转译，可以利用反斜杠来`\`插入一些在语法中有其它意义的符号，Markdown支持的转义字符列表：  
  
<div style="background-color:black;color:#fff;padding-left:20px;padding-top:10px;padding-bottom:10px;border-radius:6px;">\   反斜线<br/>
	`   反引号<br/>
*   星号<br/>
_   底线<br/>
{}  花括号<br/>
[]  方括号<br/>
()  括弧<br/>
#   井字号<br/>
+   加号<br/>
-   减号<br/>
.   英文句点<br/>
!   惊叹号</div>  

## 三、引用  
文字引用在文本前加入`>`大于号（空格）即可  
  
<div style="background-color:black;color:#fff;padding-left:20px;padding-top:10px;padding-bottom:10px;border-radius:6px;">> 引用</div>  
  
效果：  
> 引用  

多级引用嵌套需要需要这样写:  
  
<div style="background-color:black;color:#fff;padding-left:20px;padding-top:10px;padding-bottom:10px;border-radius:6px;">> 一级引用<br/>&nbsp;&nbsp;&nbsp;&nbsp;>>二级引用<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;>>>三级引用</div>  

效果：  
> 一级引用  
>>二级引用  
>>>三级引用    

## 四、列表  
1.无序列表，由`*`或`+`或`-`（空格）和文本组成，建议一个列表尽量使用其中一种符号。  

<div style="background-color:black;color:#fff;padding-left:20px;padding-top:10px;padding-bottom:10px;border-radius:6px;">+&nbsp;&nbsp;第一<br/>+&nbsp;&nbsp;第二<br/>+&nbsp;&nbsp;第三</div>  

+ 第一
+ 第二  
+ 第三  
   
2.有序列表，由数字和 . （空格）与文本组成，数字无需按大小排序，会自动编辑排序。  

<div style="background-color:black;color:#fff;padding-left:20px;padding-top:10px;padding-bottom:10px;border-radius:6px;">1.&nbsp;&nbsp;第一<br/>4.&nbsp;&nbsp;第二<br/>3.&nbsp;&nbsp;第三</div>    

1. 第一  
4. 第二  
3. 第三  

## 五、链接  
1.Inline方式，文字链接,”title”可有可无  

<div style="background-color:black;color:#fff;padding-left:20px;padding-top:10px;padding-bottom:10px;border-radius:6px;">[legendaryarthur](https://legendaryarthur.github.io 'title')</div>  

[legendaryarthur](https://legendaryarthur.github.io 'title')  

2.图片链接，大小只能借助HTML来设置,”title”可有可无  

	![karlie](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1487901610&di=bf06f3e33d576dea2dcc7e28916f68b9&imgtype=jpg&er=1&src=http%3A%2F%2Fcms-bucket.nosdn.127.net%2Fcatchpic%2F7%2F7b%2F7b28afe7586c5629dcfc8729769f8bb7.jpg%3FimageView%26amp%3Bthumbnail%3D550x0)   
  
![karlie](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1487901610&di=bf06f3e33d576dea2dcc7e28916f68b9&imgtype=jpg&er=1&src=http%3A%2F%2Fcms-bucket.nosdn.127.net%2Fcatchpic%2F7%2F7b%2F7b28afe7586c5629dcfc8729769f8bb7.jpg%3FimageView%26amp%3Bthumbnail%3D550x0 "Karlie Kloss")  

3.自动链接,尖括号(大于号、小于号)包裹地址  

<div style="background-color:black;color:#fff;padding-left:20px;padding-top:10px;padding-bottom:10px;border-radius:6px;">< https://legendaryarthur.github.io ></div>  

<https://legendaryarthur.github.io>  

4.索引链接  
<div style="background-color:black;color:#fff;padding-left:20px;padding-top:10px;padding-bottom:10px;border-radius:6px;">[legendaryarthur][1]<br/><br/>注：文档任意位置(通常是尾部)<br/>[1]:https://legendaryarthur.github.io "title"</div>  
  
[legendaryarthur][1]  
<!-- 文档任意位置(通常是尾部) -->  
[1]:https://legendaryarthur.github.io "title"  

## 六、表格  
表格是我觉得 Markdown 比较累人的地方，例子如下：  

<div style="background-color:black;color:#fff;padding-left:20px;padding-top:10px;padding-bottom:10px;border-radius:6px;">| Tables&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Are&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Cool&nbsp;&nbsp;&nbsp;&nbsp;|<br/>
| -------------&nbsp;&nbsp;&nbsp;&nbsp;|:-------------:&nbsp;&nbsp;| -----:&nbsp;&nbsp;&nbsp;&nbsp;|<br/>
| col 3 is&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| right-aligned | $1600&nbsp;&nbsp;|<br/>
| col 2 is&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| centered&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$12 |<br/>
| zebra stripes&nbsp;&nbsp;| are neat&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$1 |</div>  

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes| are neat      |    $1 |  


## 更新
1. 在表格中插入 `|` 的时候会遇到问题，这里可以用 `&#124;` 来解决；
2. 标题中如果有双引号之类的会出现错误，导致文件生成失败；
3. 实现首行缩进：
&#160;&#160;&#160;&#160;由于markdown语法主要考虑的是英文，所以对于中文的首行缩进并不太友好，两种方法都可以完美解决这个问题。
+ 把输入法由半角改为全角。 两次空格之后就能够有两个汉字的缩进。
+ 在开头的时候，先输入下面的代码，然后紧跟着输入文本即可。分号也不要掉。
```
半方大的空白&ensp;或&#8194;
全方大的空白&emsp;或&#8195;
不断行的空白格&nbsp;或&#160;
```

 
 

## 参考连接   
1. [学习Markdown](https://niuxiaokui.github.io/2016/09/23/%E5%AD%A6%E4%B9%A0markdown/)  
2. [Markdown——入门指南](http://www.jianshu.com/p/1e402922ee32/)  
3. [Markdown 语法说明 (简体中文版)](http://wowubuntu.com/markdown/#list)
