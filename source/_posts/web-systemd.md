---
title: web-systemd
date: 2018-11-04 20:59:25
categories: web
tags:
- systemd
- web

---

# systemd守护进程

在Python命令行下执行.py脚本后，在关闭命令行之前.py脚本可以保持执行状态。但是，一旦关闭命令行之后相应的脚本程序也会被终止，但是只有让py程序保持运行状态才能随时响应请求。

有多种方式可以实现对应用进程的守护，可以用linux自带的systemd实现，也可以使用专门的守护工具supervisor。

下面介绍systemd的实现方式:

# 配置

首先生成一个systemd的配置文件test.service：

```
[Unit]
Description=mushroom
After=rc-local.service

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/path for.py file
ExecStart=/usr/bin/python3 .py filename
Restart=always

[Install]
WantedBy=multi-user.target
```

# 保存位置

把此配置文件复制到systemd目录下: `cp /home/test.service /etc/systemd/system/`

这一步非常重要！

# 启动

使用以下命令启动该守护进程：

```
systemctl start test.service
```

停止该进程：

```
systemctl stop test.service
```

# 开机启动

```
$ systemctl enable test.service
# 以上命令相当于执行以下命令，把test.service添加到开机启动中
$ sudo ln -s  '/etc/systemd/system/test.service'  '/etc/systemd/system/multi-user.target.wants/test.service' 
```

即可实现开机启动。

# Reference

systemd实现python的守护进程

https://blog.csdn.net/luckytanggu/article/details/53467687

Systemd 入门教程：命令篇

http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html

Systemd 入门教程：实战篇

http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html

linux下python守护进程编写和原理理解

https://blog.csdn.net/luckytanggu/article/details/51790440