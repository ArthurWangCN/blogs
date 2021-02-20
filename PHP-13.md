---
title: PHP第一季（十三） 上传文件
date: 2017-04-08 21:27:23
tags: [PHP,李炎恢]
categories: 后台
---
>虽然大多数人认为Web只包含网页，但HTTP协议实际上可以传输任何文件，如office文档、PDF、可执行文件、AVI、压缩文件及各种其他文件类型。虽然FTP在历史上一直是向服务器上传文件的标准方式，但通过网页上传文件也逐渐流行起来。

## PHP上传配置
有一些配置指令可用于精细地调节PHP的文件上传功能。这些指令用来确定是否启用PHP的文件上传、可允许的最大上传文件大小、可允许的最大脚本内存分配和其他各种重要的资源。


|    指令    |    作用    |
|:---------:|:-----------:|
|file_uploads=on&#124;off | 确定服务器上的PHP脚本是否可以接受文件上传|
|max_execution_time=integer |PHP脚本在注册一个致命错误之前可以执行的最长时间，以秒为单位|
|memory_limit=integer |设置脚本可以分配到的最大内存，以MB为单位。这可以防止失控的脚本独占服务器内存|
|upload_max_filesize=integer |设置上传文件最大大小，以MB为单位。此指令必须小于post_max_size|
|upload_tmp_dir=string |设置上传文件在处理之前必须存放在服务器的临时一个位置，直到文件移动到最终目的地为止|
|post_max_size=integer |确定通过POST方法可以接受的信息的最大大小，以MB为单位|

## $_FILES数组
上传表单的HTML

```
<form enctype="multipart/form-data" action="upload.php" method="post">
	<input type="hidden" name="MAX_FILE_SIZE" value="1000000" />
	上传文件: <input type="file" name="userfile" />
	<input type="submit" value="上传" />
</form>
```

+ ENCTYPE="multipart/form-data"，这里是固定写法，否则文件上传失败 
+ ACTION="upload.php“，定义要处理上传的程序文件路径 

METHOD="post"，定义传输方式为POST，一般情况下Form提交数据都设置为POST 

+ `＜input type="hidden" name="MAX_FILE_SIZE" value="1000000"＞`，这是一个隐藏域，定义了上传文件的大小上限，超过这个值时，上传失败。它必须定义在文件上传域的前面.而且这里定义的值不 能超过在php.ini 文件中upload_max_filesize设置的值,否则没有意义了.（注意：MAX_FILE_SIZE 的值只是对浏览器的一个建议，实际上它可以被简单的绕过。因此不要把对浏览器的限制寄希望于该值。实际上，PHP.ini设置中的上传文件最大值，是不会 失效的。但是最好还是在表单中加上 MAX_FILE_SIZE，因为它可以避免用户在花时间等待上传大文件之后才发现该文件太大了的麻烦。） 
+ `<input type="file" name="userfile" />`,这是文件上传域,Type属性必须设置为file,但Name属性可以自定义,这个值会在代码文件中使用. 



$_FILES超级全局变量，它储存各种与上传有关的信息，这些信息对于通过PHP脚本上传到服务器的文件至关重要。

+ 存储在$_FILES["userfile"]["tmp_name"]变量中的值就是文件在Web服务器中临时存储的位置。
+ 存储在$_FILES["userfile"]["name"]变量中的值就是用户系统中的文件名称。
+ 存储在$_FILES["userfile"]["size"]变量中的值就是文件的字节大小。
+ 存储在$_FILES["userfile"]["type"]变量中的值就是文件的MIME类型，例如：text/plain或image/gif。
+ 存储在$_FILES["userfile"]["error"]变量中的值将是任何与文件上载相关的错误代码。这是在PHP4.2.0中增加的新特性。

>error分别提供了一些数组常量：0:表示没有发生错误，1:表示上载文件的大小超出了约定值。文件大小的最大值是PHP配置文件中指定的，该指令是upload_max_filesize。2:表示上载文件大小超出了HTML表单的MAX_FILE_SIZE元素所指定的最大值。3:表示文件只被部分上载。4:表示没有上载任何文件。

```
<?php
	print_r($_FILES);
?>
```

## PHP上传函数
PHP的文件系统库中提供了大量文件处理函数，除此之外，PHP还提供了两个专门用于文件上传过程的函数：is_uploaded_file()和move_uploaded_file()。

### 确定是否上传文件：is_uploaded_file()
```
if (is_uploaded_file($_FILES["userfile"]["tmp_name"])) {
	echo '已经上传到临时文件夹';
} else {
	echo '失败';
}
```

### 移动上传文件：move_uploaded_file()
```
if (!move_uploaded_file($_FILES["userfile"]["tmp_name"],$_FILES["userfile"]["name"])) {
		echo '移动失败';
		exit;
}
```