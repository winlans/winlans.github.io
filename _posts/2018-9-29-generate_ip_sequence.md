---
title: 生成指定区间的ip序列
date: 2018-9-29 21:00:00
categories:
- php
tags:
- test
---

借用php的一对函数，可以很方便的生成连续的ip序列。
<!-- more -->

# 第一个版本
> 有缺陷， 区间过大时会造成内存饱满，导致php挂掉

```php
    foreach (range(ip2long('59..1.1'), ip2long('59.42.255.255'), $step) as $ip) {
        echo long2ip($ip) . PHP_EOL;
    }
```

# 第二个版本， 使用生成器
> 即使区间再大， php的内存占用始终保持一个常量

```php
 $func = function ($startIp, $endIp, $step = 1) {
            $endIpL = ip2long($endIp);
            for ($i = ip2long($startIp); $i <= $endIpL; $i +=$step) {
                yield long2ip($i);
            }
        };
foreach ($func('192.168.1.1', '192.168.255.255') as $ip) {
    echo $ip . PHP_EOL;
}
```
