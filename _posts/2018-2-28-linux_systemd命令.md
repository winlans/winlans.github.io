---
title: linux_systemd命令
date: 2018-2-28 21:00:00
categories:
- linux
tags:
- systemd
- systemdctl
---

新版的Linux发行版都开始使用systemd来取代传统的systemv, 相比于传统的systemv， systemd更加易于配置， 统一操作。发现已经有人写的很全了， 就做个链接好了[简单入门看阮老师这篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html), 希望能够更加深入的理解的话建议看arch的文档[点这里](https://wiki.archlinux.org/index.php/systemd_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 。

<!-- more -->

已经有的就懒得重新写了。

列出来常用的几个吧

- 开启自启   `systemctl enable software_name`
- 关闭自启   `systemctl disable software_name`
- 自启列表   `systemctl list-units` 添加`--all` 查看所有可配置列表
- 是否自启   `systemctl is-enabled software_name`
- 是否启动  `systemctl is-active software_name`
