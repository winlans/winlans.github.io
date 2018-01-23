---
title: php-redis 自动序列化
date: 2018-1-21 22:00:00
tags:
- php
categories:
- php
---



有时候为了存储数组，或者对象方便，有些人会使用php-redis的自动序列化选项， 但是这有时候会很坑， 因为类似这些对象， 大多时候都是单例模式， 如果有个人在某个角落用了一下子， 后面的人再使用的是偶就会发现， redis的行为变得很古怪了。



例如我们利用redis存储session的时候， 如果这个选项被加上了， 你就会惊讶的发现， 咋回事， 为什么我存储的session会被序列化两次。。。。就是这个原因了。

```php
$redis->setOption(Redis::OPT_SERIALIZER, Redis::SERIALIZER_NONE);	// don't serialize data
$redis->setOption(Redis::OPT_SERIALIZER, Redis::SERIALIZER_PHP);	// use built-in serialize/unserialize
$redis->setOption(Redis::OPT_SERIALIZER, Redis::SERIALIZER_IGBINARY);	// use igBinary serialize/unserialize
```

