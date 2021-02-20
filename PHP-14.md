---
title: PHP第一季（十四）  处理图像
date: 2017-04-09 21:40:09
tags: [PHP,李炎恢]
categories: 后台
---
>在PHP5中，动态图象的处理要比以前容易得多。PHP5在php.ini文件中包含了GD扩展包，只需去掉GD扩展包的相应注释就可以正常使用了。PHP5包含的GD库正是升级的GD2库，其中包含支持真彩图像处理的一些有用的JPG功能。
一般生成的图形，通过PHP的文档格式存放，但可以通过HTML的图片插入方式SRC来直接获取动态图形。比如，验证码、水印、微缩图等。

## 创建图像
**创建图像的一般流程：**

+ 设定标头，告诉浏览器你要生成的MIME类型。
+ 创建一个图像区域，以后的操作都将基于此图像区域。
+ 在空白图像区域绘制填充背景。
+ 在背景上绘制图形轮廓输入文本。
+ 输出最终图形。
+ 清除所有资源。
+ 其他页面调用图像。

### 设定标头指定MIME输出类型
```
<?php
	header('Content-Type: image/png');
?>
```

### 创建一个空白的图像区域
```
<?php
	$im = imagecreatetruecolor(200,200);
?>
```

### 在空白图像区域绘制填充背景
```
<?php
	$blue = imagecolorallocate($im,0,102,255);
	imagefill($im,0,0,$blue);
?>
```

### 在背景上绘制图形轮廓输入文本
```
<?php
	$white = imagecolorallocate($im,255,255,255);
	imageline($im,0,0,200,200,$white);
	imageline($im,200,0,0,200,$white);
 	imagestring($im, 5, 80, 20, "Mr.Lee", $white);
?>
```

### 输出最终图形
```
<?php
	 imagepng($im);
?>
```

### 清除所有资源
```
<?php
	 imagedestroy($im);
?>
```

### 其他页面调用创建的图形
```
<img src="Demo4.php" alt="PHP创建的图片" />
```


## 简单小案例
### 简单验证码
```
<?php
	header('Content-type: image/png');
	for($Tmpa=0;$Tmpa<4;$Tmpa++){
		$nmsg.=dechex(rand(0,15));
	}
	$im = imagecreatetruecolor(75,25);
	$blue = imagecolorallocate($im,0,102,255);
	$white = imagecolorallocate($im,255,255,255);
	imagefill($im,0,0,$blue);
	imagestring($im,5,20,4,$nmsg,$white);
	imagepng($im);
	imagedestroy($im);
?>
```

### 加载已有的图像
```
<?php
	 header('Content-Type:image/png');
	 define('__DIR__',dirname(__FILE__).'\\');
	 $im = imagecreatefrompng(__DIR__.'222.png');
	 $white = imagecolorallocate($im,255,255,255);
	 imagestring($im,3,5,5,'http://www.yc60.com',$white);
	 imagepng($im);
	 imagedestroy($im);
?>
```

### 加载已有的系统字体
```
<?php
	$text = iconv("gbk","utf-8","亚瑟王");
	$font = 'C:\WINDOWS\Fonts\SIMHEI.TTF';
	imagettftext($im,20,0,30,30,$white,$font,$text);
?>
```

### 图像微缩
```
<?php
	header('Content-type: image/png');
	define('__DIR__',dirname(__FILE__).'\\');
	list($width, $height) = getimagesize(__DIR__.'222.png');
	$new_width = $width * 0.7;
	$new_height = $height * 0.7;
	$im2 = imagecreatetruecolor($new_width, $new_height);
	$im = imagecreatefrompng(__DIR__.'222.png');
	imagecopyresampled($im2, $im, 0, 0, 0, 0, 
$new_width, $new_height, $width, $height);
	imagepng($im2);
	imagedestroy($im);
	Imagedestroy($im2);
?>
```
