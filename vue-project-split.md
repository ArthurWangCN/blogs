---
title: split,formatDate组件
date: 2017-07-14 14:44:42
tags: Vue
categories: 前端
---
## split组件
split组件因为常使用,所以单独独立出来,并且结构相当简单。

```
<template>
  <div class="s
<script>
  export default {};
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .split
    width: 100%
    height: 16px
    border-top: 1px solid rgba(7, 17, 27, 0.1)
    border-bottom: 1px solid rgba(7, 17, 27, 0.1)
    background: #f3f5f7
</style>
```


## 日期转换组件
formatDate.js

```
vue过滤器使用
<div class="time">{{rating.rateTime | formatDate}}</div>
```

```
//在es6下,export 函数function的导入需要这样写
import { formatDate } from '../../common/js/date'; //导入自定义的date模块
```

```
//vue里面的filters
filters: {
      formatDate(time) {
        let date = new Date(time);
        //调用date模块的formatDate函数来解析时间
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    },
```
    
formatDate.js是一个自定义的js组件,不是vue组件,目录位于: `src/common/js`,这种写法是为了练习js的模块化编程

+ 将单独的一个函数写成一个模块
+ 通过export导出函数
+ 通过import导入函数

```
export function formatDate(date, fmt) { //在es6下导出一个函数
//对一个或多个y进行匹配,匹配到就进行年的替换(年有四位,所以需要特殊处理)
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length));
  }
  let o = {
    'M+': date.getMonth() + 1, //js的月是从0开始算,所以要加1
    'd+': date.getDate(),
    'h+': date.getHours(),
    'm+': date.getMinutes(),
    's+': date.getSeconds()
  };
  //对月,日,时,分,秒进行匹配替换(这些都是两位,可以一起处理)
  for (let k in o) {
    if (new RegExp(`(${k})`).test(fmt)) { //匹配到key例如MM
      let str = o[k] + ''; //然后o['MM'] 就是date.getMonth() + 1  
      //如果匹配到的时间是1位数,例如是M,那么就直接使用date.getMonth() + 1的值,
      //如果是两位数,那么就在前面补0,使用padLeftZero函数
      fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? str : padLeftZero(str)); 
    }
  }
  return fmt;
};
//先加两个0,然后再根据长度截取(因为最长也就2个0的长度)
function padLeftZero(str) {
  return ('00' + str).substr(str.length);
}
```