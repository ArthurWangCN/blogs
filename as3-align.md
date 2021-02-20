---
title: AS3：文字居中的四种办法
date: 2018-06-04 17:25:13
tags: [游戏开发, AS3]
categories: 游戏开发
---
1. Text.autoSize

	这种方法可以设置文本的对齐方法，按后计算文字长度再通过设置文本的x坐标，从而达到居中显示的目的。

	但这种方法无法在不自动换行的情况下限制文本的长度。也就是说指定txt.width属性是无效的。

	```
	myText.autoSize=TextFieldAutoSize.LEFT;
	myText.x=stage.stageWidth/2-myText.textWidth/2;```


2. TextFormat.align = "center"

	这种方法是通过指定一个TextFormat对象给文本的txt.defaultTextFormat属性。并设置这个TextFormat对象的 TextFormat.align = "center"。这种方法需要设置文本长度，文字将在指定的文本长度中处于居中位置。

	```var tf:TextFormat = new TextFormat ();
	tf.align = "center";
	myText.width = 400;
	myText.defaultTextFormat = tf;```


3. htmlText的p标签

	这种方法是在文本的txt.htmlText属性值得中设置对齐方式。在flash中支持的html标签中，p标签支持align属性。这种方法需要设置文本长度，文字将在指定的文本长度中处于居中位置。

	```
	myText.width = 400;
	myText.htmlText = "<p align='center'>文本内容...</p>";```


4. styleSheet的textAlign = "center"

	这种方法是设置文本的txt.styleSheet，在styleSheet中设置textAlign = "center"。这种方法需要设置文本长度，文字将在指定的文本长度中处于居中位置。

	```
	var style:StyleSheet = new StyleSheet();
	var p:Object = new Object();
	p.textAlign = "center";
	style.setStyle("p", p);
	myText.width = 400;
	myText.styleSheet = style;```


[本文来自网络](http://blog.163.com/z_element/blog/static/1779242202013112625840700/)
