---
title: Ubuntu启用ufw防火墙
date: 2018-1-31 14:00:00
categories:
- linux
tags:
- firewall
---

> 防火墙是计算机中一款应用软件或基于硬件的网络安全系统。它根据应用配置的规则，分析数据包，然后决定是否允许此数据包通过，来控制整个系统的网络数据进出访问权限。
>
> 常用的Linux下的防火墙的软件是iptables， 但是iptables的设置规则比较复杂， 因此ubuntu在此之上封装形成了ufw

### 安装

- 判断是否安装了ufw
  `sudo dpkg --get-selections | grep ufw`,

- 安装
  `sudo apt-get install ufw`

- 查看运行状态
  `sudo ufw status`

### 使用

- 启动
  > 默认的规则是禁止一切外部连接，允许所有外出连接。（  **包括你登陆的ssh**， 但是启动之后， 当前的连接是不会断掉的， 但是如果你 **没有将ssh添加允许的规则的话， 你就会登陆不进去**了。）

  `sudo ufw enable`

- 禁止使用
  `sudo ufw disable`

### 管理

- 查看设置的规则列表
  `sudo ufw status verbose`,  `sudo ufw status numbered`  列出序号，方便精确删除

- 添加规则
  > 有关子网掩码看[这里](https://baike.baidu.com/item/%E5%AD%90%E7%BD%91%E6%8E%A9%E7%A0%81)
  >
  > allow 默认是in , 如果想端口可以出站， 要指定out

  ```shell
  #指定端口
  sudo ufw allow 22
  # 指定端口允许的协议 tcp || udp
  sudo ufw allow 22/tcp   

  # 开放指定范围的端口, 这时候必须指定允许的协议
  sudo ufw allow 2000:2100/tcp 

  # 对指定ip开放所有端口
  sudo ufw allow from 192.168.0.104

  # 对这个ip只能使用tcp协议访问22号端口
  sudo ufw allow from 192.168.0.104 proto tcp to any port 22

  # 对于这个ip只开放22号端口
  sudo ufw allow from 192.168.0.104 to any port 22

  # 通过子网掩码限定，允许IP段192.168.1.1到192.168.1.254的所有连接
  sudo ufw allow from 192.168.1.0/24

  ```

  **拒绝连接， 简单的将allow换成deny就好了**

- 删除规则

  ```shell
  #通过序号删除，更加精准
  # 通过命令 sudo ufw status numbered 获取到序列号
  sudo ufw delete [number]

  # 通过规则删除， 与添加的时候相反
  # 删除允许22号端口入站的规则
  sudo ufw delete allow 22
  ```

- 重载配置
  `sudo ufw reload`

- 重置规则
  > 这将会把ufw的规则重置到初始状态， 但是已经设定的规则， 默认在执行的时候会被保存到配置文件目录(`/etc/ufw`)下面

  `sudo ufw reset` 

### 参考

  参考信息：

  ​	1. [Linux实用工具总结之UFW](http://notes.maxwi.com/2017/01/19/linux-command-tools-ufw/)	

  ​	2. [简单ufw](https://linux.cn/article-2489-1.html)
