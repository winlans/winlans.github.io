---
title: Linux服务器配置ssh证书登陆
date: 2018-1-31 10:00:00
categories:
- linux
tags:
- sshd_config
---

> 使用证书登陆有助于保护服务器的安全， 配置好之后， 一定要先测试下， 然后关闭密码登陆， 要是自己都进不去了， 就尴尬了。

参考文章 [ssh-证书登陆](http://www.linuxidc.com/Linux/2015-12/126648.htm)

<!-- more -->

# ssh服务器配置

## 修改sshd配置文件

> 配置文件默认存放在`/etc/ssh/sshd_config`, 如果找不到可以使用`whereis ssh` , 来查找与ssh相关的目录， 使用 `vi /etc/ssh/sshd_config` 来编辑此文件

```shell
Port 22  # 默认值22， 建议自定义
Protocol # //默认值2， 不建议1
PermitRootLogin no # 禁止使用root账户通过ssh登陆
PermitEmptyPasswords no # 禁止使用空密码登陆， 默认no
#AllowUsers user1 user2
AllowGroups ssh-group  #允许ssh登陆的用户或者组，建议使用组， 方便管理

#不让 sshd 去检查用户目录或某些重要档案的权限数据，这是为了防止用户将某些重要档案的权限设错。例如用户的 ~.ssh/ 权限设错时，某些特殊情况下会不许用户登录。
StrictModes no  # 默认yes

#允许用户自行使用成对的密钥系统登录服务器，这里仅针对协议版本 2。用户自己生成的公钥数据就放置于相应的（服务器上该用户自己的）用户目录（例如，/home/luser1）下的 .ssh/authorized_keys 文件里面，
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile %h/.ssh/authorized_keys

 # HostKeys for protocol version 2 , 根据不同的算法， 保存不同的key到相应的文件
 HostKey /etc/ssh/ssh_host_rsa_key
 HostKey /etc/ssh/ssh_host_dsa_key
 HostKey /etc/ssh/ssh_host_ecdsa_key
 HostKey /etc/ssh/ssh_host_ed25519_key

#禁用密码登录方式（不禁用密码登录的时候也可以同时用证书登录；可先不禁用，以防止自己把自己关在服务器外面了），
PasswordAuthentication no
ChallengeResponseAuthentication no

# 日志等级， 默认info， 记录基础信息， 可以使用下面的， 记录更加详细
LogLevel VERBOSE

# 允许使用 SFTP 子系统（FileZilla 要用这个）

Subsystem sftp /usr/local/ssh/libexec/sftp-server

#AuthorizedKeysFile %h/.ssh/authorized_keys, 在每个用户名下创建这个文件。将公钥放入
```

## 附加

- 创建组， 并添加组内用户

```shell
  # groupadd ssh-group
  # usermod -a -G ssh-group <username>
```

- 给用户添加public-key

  ```shell
  mkdir ~/.ssh && cd ~/.ssh && touch authorized_keys
  cat user_id_rsa.pub >> authorized_keys
  ```

- ssh登陆
  `ssh -i /path/to/private_key/id_rsa luser1@<ssh_server_ip>`

- 生成证书
  - Linux  `ssh-keygen`, 一路回车就好了， 然后`cd ~/.ssh`， 去查看生成的证书， 并将**id_rsa.pub**给服务器管理员
  - windows , 借用gitbash我们也可以使用`ssh-keygen`, 这个命令， 基本与上面相同

### 验证成功之后

成功之后， 就可以直接关闭密码登陆了。
如果是使用gitbash登陆的话， 为了方便登陆， 而不是每次输入指令，可以设置一个别名(alias),

```shell
 vi ~/.bashrc #加入下面这句话， raye 可以替换成自己想要的名字， -i后面是私钥路径 ，后面就是通用的了
 # 等号两边不要有空格， 然后就可以使用 raye， 直接登陆服务器了。
 alias raye='ssh -i ~/.ssh/id_rsa raye@192.168.1.148'
```
