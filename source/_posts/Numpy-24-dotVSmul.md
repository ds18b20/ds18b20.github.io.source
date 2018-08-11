---
title: Numpy-24-dotVSmul
date: 2018-05-19 08:55:59
categories: Python
tags:
- Numpy
- speed
- dot vs multiply
---

# Calculation speed up by Numpy

有两个矩阵a和b，二者有相同的列数。
$$
a=\left[
\begin{matrix}
a & b & c\\\
d & e & f
\end{matrix}
\right]
\\\
b=\left[
\begin{matrix}
1 & 2 & 3 \\\
4 & 5 & 6 \\\
7 & 8 & 9 \\\
10 & 11 & 12 
\end{matrix}
\right]
$$
矩阵a和b的L2距离的计算结果是一个a.shape[0] * b.shape[0]的新矩阵。
$$
\begin{array}{c|lcr}
n & \text{[1, 2, 3]} & \text{[4, 5, 6]} & \text{[7, 8, 9]} & \text{[10, 11, 12]} \\\\
\hline
[a, b, c ]  &\sqrt{ (a-1)^2+(b-2)^2+(c-3)^2} & ... & ... & ... \\\\
[d, e, f ]  & ... & ... & ... & \sqrt{ (d-10)^2+(e-11)^2+(f-12)^2} 
\end{array}
$$
具体计算时，有以下几种方式：

## 直接按照定义

```python
substract = a - b
sum_square = sum(substract**2)
sum_sqrt = sqrt(sum_square)
```

但是由于a和b的形状不同，所以计算前要进行reshape操作。

上面的例子中，a的形状为[2, 3]，b的形状为[3, 3]。要进行计算，可以把a的形状reshape为[2, 1, 3]，这样a - b的形状为[2, 3, 3]。

当a和b的尺寸过大时，用这种方法计算会带来巨大的内存消耗。

## 拆开(a-b)\*\*2

首先有
$$
(a-b)^2 = a^2 + b^2 -2ab
$$
然后
$$
\sum(a-b)^2 = \sum{a^2} + \sum{b^2} -2\sum{ab}
$$

## 继续加速计算ab

sum(ab, axis=-1)的element-wise运算仍然可以使用np.dot方法来对计算进行加速。

`a * 1 + b * 2 + c * 3`可以转换成`[a, b, c]`点乘`[1, 2, 3].T`

```python
import numpy as np
import functools
import time

def timeit(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        ret = func(*args, **kwargs)
        elapsed_time = time.time() - start_time
        print('function [{}] finished in {} ms'.format(
            func.__name__, int(elapsed_time * 1000)))
        return ret
    return wrapper

@timeit
def mul(a, b):
    row = a.shape[0]
    ret = np.sum(a.reshape(row, 1, -1) * b, axis=-1)
    return ret
    
@timeit
def mul_dot(a, b):
    ret = np.dot(a, b.T)
    return ret
    
if __name__ == "__main__":
    a = np.random.randn(50, 32*32*3)
    b = np.random.randn(500, 32*32*3)
    
    print(mul(a, b)[0][0])
    print(mul_dot(a, b)[0][0])
```

输出，

```
function [mul] finished in 435 ms
Return Array[0][0] 104.949466573
function [mul_dot] finished in 13 ms
Return Array[0][0] 104.949466573
```

