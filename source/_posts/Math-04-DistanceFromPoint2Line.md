---
title: Math-04-DistanceFromPoint2Line
date: 2018-05-25 21:34:01
categories: Math
tags:
- Distance from point to line
---

# 点到直线的距离

已知直线上的两点和直线外的一点，计算点到直线的距离。

## 向量法

可以先转换成两个向量：**a**和**b**，这时计算点到直线的距离可以先算出向量**c**的坐标表示，然后距离向量**e**=**a**-**c**，最后对向量**e**取模即可得到距离。

![](C:/Users/ds18b20/PycharmProjects/Hexo/source/_posts/Math-04-DistanceFromPoint2Line/diatance.jpg)
$$
c = acos\theta\dfrac{b} {|b|}
$$

$$
= \dfrac{(a·b)} {|b|}\dfrac{b} {|b|}
$$

https://blog.csdn.net/tracing/article/details/46563383

## 向量叉乘

平面上两个向量(**x**, **y**)叉乘得到的结果也是一个向量**z**，**z**的方向垂直于**xy**平面，模等于xy组成的平行四边形的面积。利用这一点可以很方便地计算点到直线的距离，即用平行四边形的面积除以底边的长度。
$$
d =\dfrac{ |AB\times AC|}{|AB|}
$$

$$
V_1(x_1, y_1)\times V_2(x_2, y_2) =x_1y_2 – x_2y_1
$$

https://blog.csdn.net/zxj1988/article/details/6260576

# 参考

点乘与叉乘的数学公式

http://gjunwei.blog.sohu.com/58747706.html

http://www.waitingfy.com/archives/320