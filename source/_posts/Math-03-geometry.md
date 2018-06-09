---
title: Math-03-geometry
date: 2018-04-22 00:49:14
categories: Math
tags:
- geometry
---

# 共线

判断两条线段是否共线，现在自己想到的一个方法是

```
倾角相同？
  不同---不共线
  相同---第二条线段的端点在第一条线段上？
    在---共线
    不在---不共线
```

# 相交

应用夹角的正弦坐标表示方法，可以避免计算除法。

## 参考

数学美 之 判断线段相交的最简方法

https://segmentfault.com/a/1190000004457595



# 点到直线的距离

distance from point to line

## 叉乘法

利用向量的叉乘计算两向量的夹角，进而计算点到直线的距离。

```python
def distance_point_line(point: tuple, line: np.ndarray):
    if line.ndim == 2:
        line = line[0]

    # vector 1
    x1, y1 = point[0] - line[0], point[1] - line[1]
    # vector 2
    x2, y2 = (line[2] - line[0], line[3] - line[1])
    assert (x2 ** 2 + y2 ** 2) > 0
    return abs(x1 * y2 - x2 * y1) / (x2 ** 2 + y2 ** 2) ** (1 / 2)
```

##计算范数

```python
from numpy import arccos, array, dot, pi, cross
from numpy.linalg import det, norm

# from: https://gist.github.com/nim65s/5e9902cd67f094ce65b0
def distance_numpy(A, B, P):
    """ segment line AB, point P, where each one is an array([x, y]) """
    if all(A == P) or all(B == P):
        return 0
    if arccos(dot((P - A) / norm(P - A), (B - A) / norm(B - A))) > pi / 2:
        return norm(P - A)
    if arccos(dot((P - B) / norm(P - B), (A - B) / norm(A - B))) > pi / 2:
        return norm(P - B)
    return norm(cross(A-B, A-P))/norm(B-A)
```

第二种的原理还没弄明白，需要继续学习一下。

```python
#! /usr/bin/evn python
#-*- coding:utf-8 -*-

import numpy as np
import time
import functools

def timeit(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        func(*args, **kwargs)
        elapsed_time = time.time() - start_time
        print('function [{}] finished in {} ms'.format(
            func.__name__, int(elapsed_time * 1000)))
    return wrapper
    
def distance_point_line_np(point: tuple, line: np.ndarray):
    p1 = np.array(point)
    p2 = line[0][0:2]
    p3 = line[0][2:4]
    d = np.linalg.norm(np.cross(p2-p3, p2-p1))/np.linalg.norm(p3-p2)
    
    return d
    
def distance_point_line(point: tuple, line: np.ndarray):
    if line.ndim == 2:
        line = line[0]

    # vector 1
    x1, y1 = point[0] - line[0], point[1] - line[1]
    # vector 2
    x2, y2 = (line[2] - line[0], line[3] - line[1])
    assert (x2 ** 2 + y2 ** 2) > 0
    return abs(x1 * y2 - x2 * y1) / (x2 ** 2 + y2 ** 2) ** (1 / 2)
   
@timeit   
def loop_test(func, count):
    p = (0, 0)
    line = np.array([[0,-1,2,0]])
    for i in range(count):
        func(p,line)
    
if __name__ == '__main__':
    p = (0, 0)
    line = np.array([[0,-1,2,0]])
    print(distance_point_line_np(p, line))
    print(distance_point_line(p, line))
    print('use np')
    loop_test(distance_point_line_np, 1000)
    print('use raw')
    loop_test(distance_point_line, 1000)
```

然而比较了一下运行时间，

```
use np
function [loop_test] finished in 38 ms
use raw
function [loop_test] finished in 6 ms
```

结果Numpy的方式并不快，看来用在这个场合并不合适。

## 参考

https://gist.github.com/nim65s/5e9902cd67f094ce65b0