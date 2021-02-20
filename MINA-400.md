---
title: 微信小程序访问豆瓣电影api400错误解决方法
date: 2017-07-18 09:37:44
tags: [小程序, HTTP]
categories: 前端
---
写小程序demo的时候发现发起网络请求获取豆瓣电影api数据的时候报了 `400 bad request` 的错误：

```
wx.request({
    url : "https://api.douban.com/v2/movie/in_theaters",
    data: {},
    header:{
        "Content-Type":"application/json"
    },
    success: function(res) {
        console.log(res.data);
        var data = res.data;
        currentPage.setData({
            list : data.subjects
        })
    },
});
```

![MINA-400](http://blogpic.at15cm.com/MINA-400.png)


问题的根源在于开发工具升级后，请求的header的Content-type写法变了，需要改为

```
header:{
    "Content-Type":"json"
},
```


**400 Bad Request 扩展知识：**
4xx 的响应结果表明客户端是发生问题的原因所在。
`400 Bad Request` 表示请求报文中存在语法错误。当错误发生时，需修改请求的内容后再次发送请求。另外，浏览器会像 `200 OK` 一样对待该状态码。
