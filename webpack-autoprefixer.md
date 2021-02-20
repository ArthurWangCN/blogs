---
title: webpack4.x 安装postcss-loader 配合 autoprefixer 解决css3的兼容性
date: 2020-02-26 18:40:57
tags: [webpack]
categories: [工程化, webpack]
---

## 安装

浏览器前缀需要 post-loader、autoprefixer 

```
cnpm install --save-dev css-loader style-loader postcss-loader autoprefixer
```

## 配置webpack.config.js

```
{
  test:/\.css$/,
  use:[
    {loader:'style-loader'},
    {
      loader:'css-loader',
        options:{
          importLoaders:1
        }
    },
    {loader:'postcss-loader'}
  ]
}
```

## 根目录创建postcss.config.js

在项目根目录创建 `postcss.config.js`，并且设置支持哪些浏览器，**必须设置支持的浏览器才会自动添加添加浏览器兼容**

```
module.exports = {
  plugins: [
    require('autoprefixer')({
      "browsers": [
        "defaults",
        "not ie < 11",
        "last 2 versions",
        "> 1%",
        "iOS 7",
        "last 3 iOS versions"
      ]
    })
  ]
};
```