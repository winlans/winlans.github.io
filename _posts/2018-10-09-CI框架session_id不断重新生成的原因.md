---
title: using xdebug
date: 2018-10-09 12:00:00
categories:
- php
tags:
- php
---

| 程序| 版本|
|PHP|7.2 |
|CI| 3.0|

今天在使用ci框架的时候发现，后台界面总是登陆不上， 代码中session信息也的确存储了， 后来发现，session_id在每次访问的时候都会重新生成， 这是为什么呢？百度，google了很长时间， 也没有找到问题的解决办法， 后来在代码的一处地方发现了这个校验session名字的地方。

<!-- more -->

```php
// Sanitize the cookie, because apparently PHP doesn't do that for userspace handlers
if (isset($_COOKIE[$this->_config['cookie_name']])
    && (
        ! is_string($_COOKIE[$this->_config['cookie_name']])
        OR ! preg_match('/^[0-9a-f]{40}$/', $_COOKIE[$this->_config['cookie_name']]) // 这里正则是死的
        )
    )
{
    unset($_COOKIE[$this->_config['cookie_name']]);
}

session_start();
```

产生这种问题的原因， 是php版本的差异造成的， php7.1引入了控制session_id生成的配置选项

1. session.sid_length
   > 望文生义， 这个配置就是用来配置session_id的长度的， 推荐是32位
2. session.sid_bits_per_character
   > 这个选项用来配置id中包含的字符种类， 共有三种选择  '4'（0-9，a-f），'5'（0-9，a-v），以及 '6'（0-9，a-z，A-Z，"-"，","）, 

看到这里我想你已经知道为什么会不断的生成了新的sessionid了。