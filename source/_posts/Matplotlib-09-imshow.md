---
title: Matplotlib-09-imshow
date: 2018-08-23 23:04:21
categories: Python
tags:
- Matplotlib
- Python
- imshow

---

# matplotlib imshow

用matplotlib显示cifar10数据集样本时，输出图片颜色怪异，原因在于用于显示的像素值需要转换成[0, 1]之间的浮点数。

## 直接显示raw图像：

![](Matplotlib-09-imshow\figure_1.png)

## 压缩到[0, 1]后的图像：

![](Matplotlib-09-imshow\figure_2.png)

# Reference

https://stackoverflow.com/questions/35420749/imshow-inverts-colors-after-using-astypefloat
https://stackoverflow.com/questions/24739769/matplotlib-imshow-plots-different-if-using-colormap-or-rgb-array



