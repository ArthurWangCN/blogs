---
title: hexo d 时报错 dev/tty No such device or address
date: 2018-08-09 01:44:39
tags: [hexo, 博客搭建]
categories: 博客搭建
---

提示如下，本地localhost:4000是可以打开的

```
$ hexo d
INFO Deploying: git
INFO Clearing .deploy folder...
INFO Copying files from public folder...
warning: LF will be replaced by CRLF in 2015/09/10/hello-world/index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in archives/2015/09/index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in archives/2015/index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in archives/index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in css/fonts/fontawesome-webfont.svg.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in css/style.css.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in fancybox/helpers/jquery.fancybox-buttons.css.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in fancybox/helpers/jquery.fancybox-buttons.js.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in fancybox/helpers/jquery.fancybox-media.js.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in fancybox/helpers/jquery.fancybox-thumbs.css.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in fancybox/helpers/jquery.fancybox-thumbs.js.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in fancybox/jquery.fancybox.css.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in fancybox/jquery.fancybox.js.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in fancybox/jquery.fancybox.pack.js.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in js/script.js.
The file will have its original line endings in your working directory.
On branch master
nothing to commit, working directory clean
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': No error
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': No error
```


解决：
修改 `_config.yml` ：

```
deploy:
  type: git
  repository: ssh://git@github.com/username/username.github.io
  branch: master
```


参考链接：
+ [hexo无法上传到github 试了各种办法依然无法成功，小白求助](https://segmentfault.com/q/1010000003734223)
+ [hexo deploy #1503](https://github.com/hexojs/hexo/issues/1503)