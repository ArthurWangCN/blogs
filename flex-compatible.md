---
title: CSS之flex兼容
date: 2020-02-03 00:14:02
tags: [CSS, flex]
categories: [前端, CSS]
---
flex布局分为旧版本 `dispaly: box;`，过渡版本 `dispaly: flex box;`，以及现在的标准版本 `display: flex`;。所以如果你只是写新版本的语法形式，是肯定存在兼容性问题的。

**Android**

+ 2.3 开始就支持旧版本 `display:-webkit-box;`
+ 4.4 开始支持标准版本 `display: flex;`

**IOS**

+ 6.1 开始支持旧版本 `display:-webkit-box;`
+ 7.1 开始支持标准版本 `display: flex;`

**PC**

+ ie10开始支持，但是IE10的是-ms形式的。

**兼容方法：**

```
.box{

    display: -webkit-flex;  /* 新版本语法: Chrome 21+ */
    display: flex;          /* 新版本语法: Opera 12.1, Firefox 22+ */
    display: -webkit-box;   /* 老版本语法: Safari, iOS, Android browser, older WebKit browsers. */
    display: -moz-box;      /* 老版本语法: Firefox (buggy) */
    display: -ms-flexbox;   /* 混合版本语法: IE 10 */   

 }

.flex1 {            
    -webkit-flex: 1;        /* Chrome */  
    -ms-flex: 1             /* IE 10 */  
    flex: 1;                /* NEW, Spec - Opera 12.1, Firefox 20+ */
    -webkit-box-flex: 1     /* OLD - iOS 6-, Safari 3.1-6 */  
    -moz-box-flex: 1;       /* OLD - Firefox 19- */       
}
```


本文来自 [贺贺V5](https://blog.csdn.net/u010130282/article/details/52627661)