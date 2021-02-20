---
title: JS字符串转换成数字的三种方法
date: 2017-09-11 16:38:07
tags: JavaScript
categories: 前端
---
在JS读取文本框或者其他表单数据的时候获得的值是字符串类型的,比如两个文本框 `a` 和 `b`,假设获得 a 的 value 值为 11, b 的 value 值为 9 ,那么 `a.value` 要小于 `b.value`,因为他们都是字符串形式的，这有可能不是你期望的，这时候数据类型转换就显得尤为重要。

方法主要有三种

转换函数、强制类型转换、利用js变量弱类型转换。


## 转换函数
JS提供了 `parseInt()` 和 `parseFloat()` 两个转换函数。前者把值转换成整数，后者把值转换成浮点数。仅仅有对String类型调用这些方法，这两个函数才干正确执行；对其它类型返回的都是 `NaN(Not a Number)`。

一些示例如下：

```
parseInt("1234blue"); //returns 1234
parseInt("0xA"); //returns 10
parseInt("22.5"); //returns 22
parseInt("blue"); //returns NaN
```

parseInt()方法还有基模式，能够把二进制、八进制、十六进制或其它不论什么进制的字符串转换成整数。基是由parseInt()方法的第二个参数指定的，示比例如以下：

```
parseInt("AF", 16); //returns 175
parseInt("10", 2); //returns 2
parseInt("10", 8); //returns 8
parseInt("10", 10); //returns 10
```

假设十进制数包括前导0，那么最好採用基数10，这样才不会意外地得到八进制的值。比如：

复制代码 代码例如以下:

```
parseInt("010"); //returns 8
parseInt("010", 8); //returns 8
parseInt("010", 10); //returns 10
```

parseFloat()方法与parseInt()方法的处理方式相似。
使用parseFloat()方法的还有一不同之处在于，字符串必须以十进制形式表示浮点数，parseFloat()没有基模式。

以下是使用parseFloat()方法的演示样例：

复制代码 代码例如以下:

```
parseFloat("1234blue"); //returns 1234.0
parseFloat("0xA"); //returns NaN
parseFloat("22.5"); //returns 22.5
parseFloat("22.34.5"); //returns 22.34
parseFloat("0908"); //returns 908
parseFloat("blue"); //returns NaN
```

## 强制类型转换
还可使用强制类型转换（type casting）处理转换值的类型。使用强制类型转换能够访问特定的值，即使它是还有一种类型的。

ECMAScript中可用的3种强制类型转换例如以下：
Boolean(value)——把给定的值转换成Boolean型；
Number(value)——把给定的值转换成数字（能够是整数或浮点数）；
String(value)——把给定的值转换成字符串。

用这三个函数之中的一个转换值，将创建一个新值，存放由原始值直接转换成的值。这会造成意想不到的后果。

当要转换的值是至少有一个字符的字符串、非0数字或对象时，Boolean()函数将返回true。假设该值是空字符串、数字0、undefined或null，它将返回false。

能够用以下的代码段测试Boolean型的强制类型转换。

```
Boolean(""); //false – empty string
Boolean("hi"); //true – non-empty string
Boolean(100); //true – non-zero number
Boolean(null); //false - null
Boolean(0); //false - zero
Boolean(new Object()); //true – object
```

Number()的强制类型转换与parseInt()和parseFloat()方法的处理方式相似，仅仅是它转换的是整个值，而不是部分值。示比例如以下：

```
Number(false) 0
Number(true) 1
Number(undefined) NaN
Number(null) 0
Number( "5.5 ") 5.5
Number( "56 ") 56
Number( "5.6.7 ") NaN
Number(new Object()) NaN
Number(100) 100
```

最后一种强制类型转换方法String()是最简单的，示比例如以下：

```
var s1 = String(null); //"null"
var oNull = null;
var s2 = oNull.toString(); //won't work, causes an error
```


## 利用JS变量弱类型转换
举个小样例，一看就会明确了。

```
<script>
var str= '012.345 ';
var x = str-0;
x = x*1;
</script>
```

上例利用了JS的弱类型的特点，仅仅进行了算术运算，实现了字符串到数字的类型转换，只是这种方法还是不推荐的