---
title: RaspberryPi-00-Installation
date: 2019-06-23 14:34:27
categories: Raspberry Pi
tags:
- Raspberry Pi
- installation
- setup

---

# Download

# 版本

# 选择版本

树莓派原生系统创建在Debian系统之上，安装文件可以在https://www.raspberrypi.org/downloads/上找到。

首先有多版本的linux可以适配树莓派的硬件，仅linux就有原生的Raspbian，Ubuntu等多个版本，另外还有Windows10可以使用。

本文就原生的Raspbian来记录一下安装过程。

# 准备工作

首先，准备硬件：

- 树莓派一块
- HDMI线和显示器
- 键盘和鼠标
- 16GB容量的SD卡
- 5V/2A的microusb接口电源

然后，准备文件：

- 下载SD格式化工具(Windows自带的也可以？)

  https://www.sdcard.org/chs/index.html

- 下载镜像写入工具

  https://sourceforge.net/projects/win32diskimager/

- 下载版本Raspbian Stretch with desktop得到zip文件，解压后得到.img的镜像文件

  https://www.raspberrypi.org/downloads/raspbian/

# 安装

1. 使用SD格式化工具将SD卡格式化为FAT32格式。

2. 使用Win32DiskImager将解压得到的img镜像文件写入SD卡中，把SD卡插到树莓派的插槽内。
3. 给树莓派插上HDMI线连接到显示器，插上鼠标键盘。
4. 插上电源开机

安装过程中会要求设置语言和时区，还有确认显示器的显示效果。

