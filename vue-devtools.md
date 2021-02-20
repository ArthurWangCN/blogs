---
title: vue-devtools 安装
date: 2017-04-26 09:35:13
tags: [Vue, 工具]
categories: 前端
---
>Vue.js devtools是基于google chrome浏览器的一款调试vue.js应用的开发者浏览器扩展。做前端开发的IT工程师应该比较熟悉这款工具，可以边侧边栏窗格中的页面，边检查代码。

github下载地址：<https://github.com/vuejs/vue-devtools>

如果你有办法 "看外面的世界"，推荐直接在 [Chrome Web Store](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd) 上面安装，真的是特别简单。

如果你看不了外面的世界，那么还是老老实实手动安装吧：

1. 下载好后进入vue-devtools-master工程
2. 执行 `npm install ----->npm run build`
3. 修改 `mainifest.json` 中的 `persistant` 为 `true`
4. 打开谷歌浏览器 `设置` --> `扩展程序` --> 勾选 `开发者模式` -->添加工程中的 `shells` --> `chrome` 的内容，至此恭喜已经安装成功！！！
5. 打开自己的vue项目中，如果是有vue-cli构建的项目，执行 `npm run dev`,打开 `http://localhost:8080/` 服务器调试地址；至此完成devtools的安装

![vue-devtools](http://blogpic.at15cm.com/vue-devtools.png)