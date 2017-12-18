---
title: windows下PHP安装 imagick
date: 2017-12-12 12:00:00
categories:
- php
tags:
- other
---

之前安装php imagick扩展的时候，死活装不上，最后被逼在linux下装了，但是今天一试居然撞上了。哈哈哈

#### 下载扩展

从php的pecl库下载imagick扩展，要对应上自己的 php version， ts or nts , x86 or x64 。确定好就可以下载了，我下载的是`php_imagick-3.4.3-5.6-ts-vc11-x64`  。 下载传送门 [php-imagick](https://pecl.php.net/package/imagick)

![imagick-1](/assets/images/postImages/imagick-1.png)

点击图中 1位置，我们可以查看这个版本扩展的详情，里面会有如下的介绍

![imagick-2](/assets/images/postImages/imagick-2.png)

这里表明了，我们安装的必须环境。

下载好对应的扩展之后，解压出来，按下图的位置放好， 以CORE_开头的放在php的根目录下，php_imagick.*放在php的扩展目录下。

![imagick-3](/assets/images/postImages/imagick-3.png)

![imagick-4](/assets/images/postImages/imagick-4.png)

打开php.ini, 添加`extension=php_imagick.dll`



#### 安装imagick软件

imagick 扩展必须配合imagick软件才能使用，下载传送门[imagick.exe](http://www.imagemagick.org/script/download.php#windows)

根据第二张图选择合适的版本，下载，我下载的是`ImageMagick-7.0.7-15-Q8-x64-dll.exe`

安装的时候不要将添加环境变量的选项打掉，不然你还要自己去添加。

安装完之后，重启web服务器，浏览phpinfo，查看是否安装完成。



就这么些，但是估计如果运气不好，可能还是装不上。。。。。。