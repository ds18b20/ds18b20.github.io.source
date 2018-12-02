---
title: TensorFlow-02-useGPUs
date: 2018-04-08 11:01:20
categories: TensorFlow
tags:
- GPU
- Setting
---

# TensorFlow使用GPU

没有指定使用CPU还是GPU的时候，TF默认寻找机器中的第一个GPU，并且占用该GPU的所有显存资源。如果没有找到GPU那就使用第一个CPU来运算。

> TensorFlow对于设备的表示：”/cpu:0”表示第一块CPU，”/gpu:0”表示第一块GPU，其他情况以此类推。

要显示系统的GPU设备时，可以运行NVIDIA自带的nvidia-smi.exe来查看。一般在路径：`C:\Program Files\NVIDIA Corporation\NVSMI`下可以看到。

```
C:\Program Files\NVIDIA Corporation\NVSMI>nvidia-smi.exe
Sun Apr 08 10:51:55 2018
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 388.13                 Driver Version: 388.13                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name            TCC/WDDM | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 960    WDDM  | 00000000:01:00.0  On |                  N/A |
| 30%   30C    P8    10W / 120W |    267MiB /  2048MiB |      9%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1932    C+G   C:\Windows\system32\Dwm.exe                N/A      |
|    0     10660    C+G   ...6)\Google\Chrome\Application\chrome.exe N/A      |
+-----------------------------------------------------------------------------+
```

# 使用多GPU

ing

# 手动分配GPU资源

ing

# 参考

TensorFlow中设置GPU

http://hpzhao.com/2016/10/29/TensorFlow%E4%B8%AD%E8%AE%BE%E7%BD%AEGPU/