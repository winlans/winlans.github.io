---
title: Multipart/form-data
date: 2017-12-04 18:00:00
categories:
- php
tags:
- fileType
---

服务器类软件，类似Apache, Nginx，在我们通过`Multipart/form-data` 上传文件的时候，是通过上传信息中的`content-Type`  来识别的，所以如果我们能自定义的话，也能绕过服务器的文件检测了，但是这个好像并没有什么意义。 简书上有篇文章已经很详细了，就不重复写了。附上链接[简书LEE](http://www.jianshu.com/p/e810d1799384)



```php
------WebKitFormBoundaryMQ9BG4Er5M1QNuj7
Content-Disposition: form-data; name="file"; filename="DeepinScreenshot_select-area_20171204212343.png"
Content-Type: image/png


------WebKitFormBoundaryMQ9BG4Er5M1QNuj7
Content-Disposition: form-data; name="form"

1
------WebKitFormBoundaryMQ9BG4Er5M1QNuj7
Content-Disposition: form-data; name="pdid"

2065
------WebKitFormBoundaryMQ9BG4Er5M1QNuj7
Content-Disposition: form-data; name="share"

[object Object],[object Object],[object Object]
------WebKitFormBoundaryMQ9BG4Er5M1QNuj7
Content-Disposition: form-data; name="session_id"

kfucrtci4alr0ak1d1324c5lg6
------WebKitFormBoundaryMQ9BG4Er5M1QNuj7--
```

