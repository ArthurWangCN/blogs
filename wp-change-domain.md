---
title: 如何完美更换WordPress网站的域名
date: 2020-04-25 21:53:44
tags: 博客搭建
categories: 博客搭建
---

不管是个人网站还是企业网站，一般我们都不建议更换网站域名，因为这不但会影响网站在搜索引擎结果中的排名，减少网站的访问量，同时还会在网站用户中留下不好印象。不过，在有些情况下，我们也必须更换域名。比如，我们购买到了更适合的域名；或者以前的域名忘记续费，不得已更换新域名；或者在网站备案期间使用的临时域名，备案后切换到自己的域名；或者我们给客户做的网站，测试完成后要切换到正式的域名等等。



我们以手头的演示网站为例，介绍一下如何将WordPress网站的域名从旧域名 www.mydomain.com 更换为新域名 www.newdomain.com 。



**第一步**，开始之前，请先做好网站的备份，备份好网站数据库和网站文件。尤其是数据库，一定要做好备份，以防操作过程中出现错误，我们可以使用备份的数据库重新进行操作。



**第二步**，将新域名做好解析和绑定操作。解析新域名，就是将域名指向服务器的IP地址，通常在域名商那里进行操作；绑定新域名，通常在空间商那里进行操作，就是在服务器上添加新域名，并确保网站目录和旧域名的网站目录一致。

完成以上两步之后，需要确认新域名生效之后，再继续进行以下操作。新域名设置解析后，通常需要一段时间才能传递到各地网络，各地生效时间并不一致，通常需要几分钟或者几个小时，最多不会超过48小时。你可以使用ping命令来检查，来查看新域名是否生效。如果ping出来的ip地址是刚刚设置的ip，那么解析就生效了。

新域名生效之后，这个时候在浏览器中输入新域名和旧域名，都可以打开原来的网站。如果旧域名已经失效，比如说已经过期，或者已经解析到其他地方等，那么网站虽然可以打开，但网页看起来会比较乱；这是因为网页无法正常加载WordPress主题的样式表。



**第三步**，登录主机管理系统，进入phpmyadmin，选择WordPress网站所使用的数据库。如果你不确定WordPress使用的是哪一个数据库，可以查看WordPress目录下的wp-config.php配置文件，查看其中的 DB_NAME 设置。

选中该数据库之后，点击SQL，输入以下代码：

```sql
UPDATE wp_posts SET post_content = replace(post_content, 'www.mydomain.com','www.newdomain.com') ;
UPDATE wp_comments SET comment_content = replace(comment_content, 'www.mydomain.com', 'www.newdomain.com') ;
UPDATE wp_comments SET comment_author_url = replace(comment_author_url, 'www.mydomain.com', 'www.newdomain.com') ;
```

 以上代码中，[www.mydomain.com](http://www.mydomain.com/) 代表原来的域名，www.newdomain.com 代表新域名。域名一定要输入完整；如果你使用类似 blog.newdomain.com 这样的二级域名，也是可以的，只要输入完整域名就可以了。 



与直接在WordPress的管理后台修改域名相比，今天介绍的这个办法有两个优点：

1. 即便旧域名已经失效了，也可以更换新域名；因为整个操作过程中，根本不需要登陆WordPress的管理后台。

2. 更换比较彻底，不光更换了网站的域名，连文章内部的链接，图片和音视频等媒体文件的地址、链接，以及评论中的链接等，都一起进行了更换。



本文参考：[https://wpchina.org/how-to-change-wordpress-domain-prefectly-1528/](https://wpchina.org/how-to-change-wordpress-domain-prefectly-1528/)

