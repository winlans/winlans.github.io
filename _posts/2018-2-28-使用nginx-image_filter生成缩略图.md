---
title: 使用nginx-image_filter生成缩略图
date: 2018-2-28 19:00:00
categories:
- nginx
tags:
- nginx
- image_filter
---



如果缩略图的尺寸比较多， 并且不想使用程序去生成， 那么就可以借助nginx的`image_filter`模块帮助我们生成。但是这有个缺点， 就是会占用nginx的资源， 如果图片很大，资源占用会更大， 但是这仍不失为是一种好的解决方案， 况且我们可以利用nginx的代理功能生成本地资源， 这样就会只生成一次， 对nginx的压力大大减小。参考了[这里](https://www.centos.bz/2017/03/using-nginx-image_filter-resize-images/)， 原文由于使用的是http， 并不适用于我们的规则， 于是稍作修改。

### 开启image_filter模块

编译参数加入`--with-http_image_filter_module`, 即可开启， [详情点击](https://www.nginx.com/resources/admin-guide/nginx-https-upstreams/) 。

- 编译nginx
  1. 使用 `nginx -V` 查看上一次的编译配置，使用` ./configure --help`, 可以查看支持的配置项, 并将 `--with-http_image_filter_module`追加其后
  2. `sudo make && sudo make install`

image_filter 模块依赖 `libgd`， 因此我们需要安装这个库

- ubuntu 

  查看是否安装 `dpkg --list | grep "libgd"`

  安装  `sudo apt-get install libgd3`

### 修改nginx配置

> 找到需要修改的域名配置文件， 然后加入下面的配置

```nginx
server {
        listen 127.0.0.1:80;

        location /image-resize {
                root /path/to/you_site_root;
            rewrite /(image-resize)/(.*) /$2 break;

        	#expires 30d;   #是否设置过期时间
            image_filter crop $arg_width $arg_height; #除了crop 还有 resize  rotate
            image_filter_jpeg_quality 75;
                image_filter_buffer 10M;  # 允许的最大图片大小， 超过会响应415
                allow 127.0.0.1/8;
                deny all;
                access_log off;
        }
}

server{
  # 开启了https的站点
 location ~* /(.*)\/(\w+)_(\d+)x(\d+)\.(jpg|png|jpeg|gif)$ {
            if (-f $request_filename) {
                        break;
            }

            set $filepath $1;
            set $filename "$2.$5";
            set $thumb    "$2_$3x$4.$5";
            set $width    $3;
            set $height   $4;
			
  			# 没找到源文件响应404
            if (!-f $document_root/$filepath/$filename) {
                        return 404;
            }

    		# 重写url到缩略图路径
            rewrite /(.*)\/([\w_]+)\.(.*) /imgcache/$2.$3;

  			# 请求的缩略图不存在，转发生成并存储
            if (!-f $request_filename) {
                        proxy_pass http://127.0.0.1/image-resize/$filepath/$filename?width=$width&height=$height;
                        break;
            }

            proxy_store          $document_root/imgcache/$thumb;
            proxy_store_access   user:rw  group:rw  all:r;
            proxy_set_header     Host $host;
            access_log off;  #关闭访问日志
        }
}
```



ps: 我觉的这样的设计或许并不完美， 相应所有的尺寸请求，容易生成大量的无用缩略图， 如果有人恶意使用中方式， 生成大量的缩略图会占用大量的存储空间。 可以修改匹配规则， 只生成特定的几种缩略图尺寸， 比如mini、medium、large...等等， 也可以再加上设备类型， 以生成不同的缩略图。

这种方式或许有缺陷， 但它大量减少了我们为了生成缩略图的编码量， 但如果生成的缩略图要求较高的话， 就难以满足了， 不过可以使用另一种解决方案`nginx +lua` 来实现。