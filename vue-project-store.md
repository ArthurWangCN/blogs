---
title: 存储读取组件
date: 2017-07-14 15:40:41
tags: Vue
categories: 前端
---
**store.js组件**

这是一个js模块,负责html5的localstoage存储和读取的,位置: `src/common/js/store.js`

使用的方法是:

```
//在es6下,export 函数function的导入需要这样写
 import { saveToLocal, loadFromLocal } from '../../common/js/store';
```

store.js

```
//存储
//传入三个参数,seller的id,要存储的key和value
export function saveToLocal (id, key, value) {
//需要加上window对象来使用localstorage
  let seller = window.localStorage.__seller__; //使用__只是一种标记写法,标记是自定义的某种编码规范,这里代表这只是seller的数据
  if (!seller) { //第一次生成seller的时候初始化
    seller = {}; 
    seller[id] = {};
  } else {
    seller = JSON.parse(seller); //json字符串需要解析
    if (!seller[id]) { //不同seller的时候初始化
      seller[id] = {};
    }
  }
  seller[id][key] = value; //生成当前的seller对象
  //localStorage只能存储字符串,需要转成json字符串
  window.localStorage.__seller__ = JSON.stringify(seller);
}
//读取
三个参数,seller的id,之前存储的key,和一个默认值
export function loadFromLocal (id, key, def) {
  let seller = window.localStorage.__seller__;
  if (!seller) { //读取不到返回默认值
    return def; 
  }
  seller = JSON.parse(seller)[id]; //json解析
  if (!seller) { //解析失败返回默认值
    return def;
  }
  let ret = seller[key]; 
  return ret || def; //解析成功但是没有这个seller的id的也返回默认值
}
```


注意：

+ 在node里面,没有默认全局window对象,所以需要指定加上才能使用window的相关方法和属性
+ seller[id][key] = value; 相当于是某个id的seller的某个属性(key)和值(value)保存为一个对象
+ 关于写入的逻辑:先读取localstorage的已有值,判断是否存在,然后再去解析localstoage的已有值,判断是否等于当前的数据的key值(id),最后再处理最终的值是否存储,这里逻辑需要先判断已有值.
+ 关于读取的逻辑:先读取localstorage判断是否有值,然后再去判断解析localstoage读取得到的值,最后再处理最终得到的值是否正常,按顺序进行逻辑处理
