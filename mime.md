---
title: 什么是MIME
date: 2018-08-31 00:03:35
tags: 前端
categories: 前端
---

MIME (Multipurpose Internet Mail Extensions) 是描述消息内容类型的因特网标准。在HTTP中，MIME类型被定义在Content-Type header中。

MIME 消息能包含文本、图像、音频、视频以及其他应用程序专用的数据。


例如，架设你要传送一个Microsoft Excel文件到客户端。那么这时的MIME类型就是“application/vnd.ms-excel”。在大多数实际情况中，这个文件然后将传送给Execl来处理（假设我们设定Execl为处理特殊MIME类型的应用程序）。在ASP中，设定MIME类型的方法是通过Response对象的ContentType属性。

最早的HTTP协议中，并没有附加的数据类型信息，所有传送的数据都被客户程序解释为超文本标记语言HTML 文档，而为了支持多媒体数据类型，HTTP协议中就使用了附加在文档之前的MIME数据类型信息来标识数据类型。

MIME意为多目Internet邮件扩展，它设计的最初目的是为了在发送电子邮件时附加多媒体数据，让邮件客户程序能根据其类型进行处理。然而当它被HTTP协议支持之后，它的意义就更为显著了。它使得HTTP传输的不仅是普通的文本，而变得丰富多彩。

每个MIME类型由两部分组成，前面是数据的大类别，例如声音audio、图象image等，后面定义具体的种类。


官方的 MIME 信息是由 Internet Engineering Task Force (IETF) 在下面的文档中提供：

+ RFC-822 Standard for ARPA Internet text messages
+ RFC-2045 MIME Part 1: Format of Internet Message Bodies
+ RFC-2046 MIME Part 2: Media Types
+ RFC-2047 MIME Part 3: Header Extensions for Non-ASCII Text
+ RFC-2048 MIME Part 4: Registration Procedures
+ RFC-2049 MIME Part 5: Conformance Criteria and Examples

不同的应用程序支持不同的 MIME 类型。


按照内容类型排列的 Mime 类型列表 见 [W3school](http://www.w3school.com.cn/media/media_mimeref.asp)

参考： [MIME是什么](https://zhidao.baidu.com/question/202397287.html)