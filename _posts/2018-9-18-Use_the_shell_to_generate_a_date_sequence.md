---
title: Use the shell to generate a date sequence
date: 2018-9-18 13:00:00
categories:
- linux
tags:
- shell
---

> 在shell中我们有时候可能需要生成一些日期序列， 下面简单的实现了一下。

<!-- more -->

```shell
#!/bin/bash
st_ti=$(date -d "$1" "+%s")
ed_ti=$(date -d "$2" "+%s")

for day in  `seq $st_ti 86400 $ed_ti`;do
    echo `date -d @$day "+%Y%m%d"`
done
```
