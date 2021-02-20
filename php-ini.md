---
title: MAC OS 下自带php.ini配置
date: 2017-04-15 10:21:57
tags: PHP
categories: 后台
---
如果你已经有 `/private/etc/php.ini` 就不需要再拷贝一份php.ini.default出来了。

```
cd /private/etc/
sudo cp php.ini.default php.ini
sudo vi php.ini
```

把php.ini里面所有的default_socket都改成/tmp的mysql.sock的正确位置即可。
因为mysql的默认目录是 `/tmp/mysql.sock.`,命令如下： 

```
方法1：echo "show variables" | mysql | grep "socket"
方法2：echo "status" | mysql | grep "socket"
```

```
pdo_mysql.default_socket=/tmp/mysql.sock
mysql.default_socket = /tmp/mysql.sock
mysqli.default_socket = /tmp/mysql.sock		
```

## 扩展
**各操作系统中php.ini的位置**

|OS |	PATH|
|:---:|:---|
|Linux |	/etc/php.ini<br/>/usr/bin/php5/bin/php.ini<br/>/etc/php/php.ini<br/>/etc/php5/apache2/php.ini|
|Mac OSX |	 /private/etc/php.ini|
|Windows (with XAMPP installed) 	| C:/xampp/php/php.ini|