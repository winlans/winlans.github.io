---
title: php安装amqp|rabbitmq
date: 2017-12-25 19:00:00
categories:
- php
tags:
- php_ext
---

首先去官网下载对应的安装包，比如我的php版本是5.6，非线程安全，x64 版本，那么我就下载 [1.4](https://pecl.php.net/package/amqp/1.4.0/windows)。
下载完之后，我们解压文件，将**rabbitmq.1.dll**放在windows的**C:\Windows\System32**下， 将**php_amqp.dll**放在php的**ext**目录下， 然后去php.ini引入amqp扩展，重启服务器，就好了。
