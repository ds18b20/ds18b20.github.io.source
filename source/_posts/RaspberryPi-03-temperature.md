---
title: RaspberryPi-03-temperature
date: 2019-06-30 13:22:50
categories: Raspberry Pi
tags:
- Raspberry Pi
- cpu
- temperature

---

# raspberry pi CPU temperature

有两种方法可以查看CPU温度：

1. /opt/vc/bin/vcgencmd measure_temp

   temp=41.3'C

2. cat /sys/class/thermal/thermal_zone0/temp

   43470

   注意，这里的单位是1/1000摄氏度

另外可以用代码实现对温度的实时监控，C语言和Python的实现可以参考一下连接：

https://blog.csdn.net/xukai871105/article/details/38349209

