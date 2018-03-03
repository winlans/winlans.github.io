---
title: windows 下常用的shell命令
date: 2018-3-3 11:00:00
categories:
- shell
tags:
- shell
---

windows下的shell是出了名的难用， 但是没办法， gui除了mac就是windows，Linux的图形操作还是不太友好。因此， 为了方便windows下的常用命令我们还是需要了解一些的。

# 查看帮助

在每个命令的后面使用 `/?` 查看命令的帮助, 例如 `netstat /?`
ps: 还是linux的`man`好用

# 端口

>linux 下的端口占用使用 `lsof` 可以方便的查看， windows 则使用 `netstat`

- 查看某个端口占用 `netstat -ano | findstr "prot_num"`

# 文本与搜索

- 查看文件内容
  1. type 类似Linux的`cat`
  2. more 与Linux的`more`相同
- 搜索字符串
  > 类似Linux的`grep` window有`findstr`
通过管道符筛选输出内容  `| findstr "search_txt"` , 具体使用查看帮助 `findstr /?`

- 快速想添加字符串到文本末尾
  `echo "some text" >> /path/to/file`
- 快速清空文本内容
  `echo "" > /path/to/filen`
  从上面我们可以看出， windows的管道操作与Linux是差不多的。

# 文件操作

> 支持通配符 *
- 移动 `move origin dest`
- 复制 `xcopy`
- 删除文件夹所有内容 `del /f /s /q  dir_name` -> `rm -rf dir_name`
- 不删除子目录 `del /f dir_name` 只删除 dir_name 下面我的文件， 子目录不会处理
- 列出目录内容 `dir /path/to/dir`

# 进程管理

- 列出所有进程 `tasklist`
- 根据进程id杀死进程 `taskkill /pid /f program_pid`
- 根据进程名杀死进程 `taskkill /im notepad.ex`
- 打开任务管理器 `taskmgr`

# 服务管理

- 列出所有服务 `service`
- 查找apache服务 `service | findstr "apache"`
- 打开服务管理gui `services.msc`

# 计划任务

> 类似Linux的crontab， 不过显然没有crontab好用
at 具体使用`/?` 查看帮助

# 其他

- 清空屏幕 `cls`
- 退出当前窗口 `exit`
- 查看系统信息 `systeminfo`
