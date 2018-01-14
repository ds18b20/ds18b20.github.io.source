---
title: KalmanFilter-01-Covariance
date: 2018-01-11 22:59:29
categories: Kalman Filter
tags:
- Kalman Filter
- Covariance
---

# 从方差说起

首先回顾一下统计学中最常见的几个概念：均值，标准差，方差。

- 均值
  $$
  \bar{X}=\frac{\sum_{i=1}^n  X_{i}}{n}
  $$
  ​

- 标准差
  $$
  s=\sqrt{\frac{\sum_{i=1}^n (X_{i}-\bar{X})^2}{n-1}}
  $$
  ​

- 方差
  $$
  s^2=\frac{\sum_{i=1}^n (X_{i}-\bar{X})^2}{n-1}
  $$





样本均值X是总体均值U的最优估计值。虽然可以用其他样本统计量来估计总体的均值，比如样本中位数，众数或中列数。但是样本均值通常能提供最优的估计性。理由之一，多个样本的均值拥有比其他统计量更好的一致性（更小的标准差）。理由之二，样本均值X是总体均值U的一个无偏估计量，这意味着样本均值分布的中心趋近于总体均值的中心。（参考《初级统计学》著者: 特里奥拉）

标准差表示样本中各点到均值的距离的平均，即可以用来评价样本数据的离散程度。标准差越大意味着，离散程度越大，数据分布不集中。标准差公式中分母使用n-1而不是n，这样可以以较小的样本集来更好地逼近总体的标准差，即统计学上的“无偏估计”。方差=标准差的平方

# 协方差的意义

通过上面对方差的介绍，可以注意到方差（标准差）只能用来描述一维数据。而具体环境中遇到的数据通常是二维甚至是多维度的，这时候描述这组数据就不应该只是单独计算各组数据的方差，因为各组数据之间很可能包含某种程度的关联。可以通过`协方差`来反映这种相关关系的程度。

我们已经知道方差的定义如下：
$$
var(x)=\frac{\sum_{i=1}^n (X_{i}-\bar{X})^2}{n-1}
$$
拆开平方，
$$
var(x)=\frac{\sum_{i=1}^n (X_{i}-\bar{X}) (X_{i}-\bar{X})}{n-1}
$$
再把分子的后项换成第二维度的Y，得到
$$
var(x,y)=\frac{\sum_{i=1}^n (X_{i}-\bar{X}) (Y_{i}-\bar{Y})}{n-1}
$$
※顺便说一下方差的英文是variance，协方差的英文是covariance。

 由上述定义可知，
$$
cov(X,X) = var(X)
$$

$$
cov(X,Y) = cov(Y,X)
$$

# 协方差矩阵

单个协方差用于表示两个维度间的相关关系，当维度增多时就需要多个协方差。数据集的维度为n时，就需要$\frac{n*(n-1)}{2}$个协方差。这些协方差通过矩阵来组织时，就称为协方差矩阵。
$$
C_{n\times n}=(c_{i,j},c_{i,j}=cov(Dim_{i},Dim_{j}))
$$

$$
C = 
\begin{pmatrix}
cov(x,x) & cov(x,y) & cov(x,z) \\
cov(y,x) & cov(y,y) & cov(y,z) \\
cov(z,x) & cov(z,y) & cov(z,z) \\
 \end{pmatrix}
$$
在此也可看到，协方差矩阵是个对称矩阵，且其对角线上是各维度上的方差。

Py代码如下：

```python
# 验证ndArray协方差矩阵
import numpy as np

sample_num = 10
sample_dim = 3
sample = np.random.randn(sample_num, sample_dim)

dim0 = sample[:,0]
dim1 = sample[:,1]
dim2 = sample[:,2]

dim0_mean = np.average(dim0)
dim1_mean = np.average(dim1)
dim2_mean = np.average(dim2)

cov01 = ((dim0 - dim0_mean) * (dim1 - dim1_mean)).sum() / (sample_num - 1)
cov02 = ((dim1 - dim1_mean) * (dim2 - dim2_mean)).sum() / (sample_num - 1)
cov12 = ((dim1 - dim1_mean) * (dim2 - dim2_mean)).sum() / (sample_num - 1)

print(cov01,cov02,cov12)
```

# 参考

浅谈协方差矩阵http://pinkyjie.com/2010/08/31/covariance/

在线LaTeX编辑[https://www.cnblogs.com/GarfieldEr007/p/5577727.html](http://latex.codecogs.com/eqneditor/editor.php)