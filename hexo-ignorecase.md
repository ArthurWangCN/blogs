---
title: Hexo 部署到 Github Pages 文件夹大小写问题 
date: 2017-07-14 15:49:16
tags: [hexo, 博客搭建]
categories: 博客搭建
---
## 问题
使用 Hexo 部署博客到 Github Pages 时经常会遇到文件夹大小写问题导致的 404 问题。

譬如 Hexo 生成了一个 `Hackerrank in JS Category` 文件夹，但是我后来把它改成了 `Hackerrank In JS`，即 in 的首字母大写了。Hexo会生成正确，但部署到 Github 上却老是不正确。


## 原因
git 默认忽略文件名大小写，所以即使文件夹大小写变更，git 也检测不到。


## 解决办法
1.进入到博客项目中 `.deploy_git` 文件夹，修改 `.git` 下的 `config` 文件，将 `ignorecase=true` 改为 `ignorecase=false`

```
cd .deploy_git
vim .git/config
```


2.删除博客项目中 `.deploy_git` 文件夹下的所有文件，并 push 到 Github 上, 这一步是清空你的 github.io 项目中所有文件。

```
git rm -rf *
git commit -m 'clean all file'
git push
```

3.使用 Hexo 再次生成及部署

```
cd ..
hexo clean
hexo deploy -generate
```