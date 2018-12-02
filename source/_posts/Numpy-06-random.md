---
title: Numpy-06-random
date: 2017-11-15 21:36:32
categories: Python
tags:
- Numpy
- Python
- random
---

# Seed

计算机生成的随机数并**不是真正的**随机数，因为这些随机数都是根据一定的计算方式得到的。

1. 生成随机数时需要指定随机种子(Seed)，当这些种子不变时，得到的随机数也不变。

```python
>>> import numpy as np

>>> np.random.seed(123)
>>> np.random.rand(2)
array([ 0.69646919,  0.28613933])
# 每次重新生成随机数之前都需要更新种子
>>> np.random.seed(123)
>>> np.random.rand(2)
array([ 0.69646919,  0.28613933])
```

2. 如果用户不提供种子，默认由系统时钟（即定时/计数器的值） 提供种子。
3. 随机数的生成算法和操作系统有关，即使给定相同的种子，在Windows和Linux下会得到不同的随机数。

# 简单随机数

| 函数名称                          | 函数功能                | 参数说明                                                     |
| --------------------------------- | ----------------------- | ------------------------------------------------------------ |
| rand(d0, d1, …, dn)               | 产生均匀分布的随机数    | dn为第n维数据的维度                                          |
| randn(d0, d1, …, dn)              | 产生标准正态分布随机数  | dn为第n维数据的维度                                          |
| randint(low[, high, size, dtype]) | 产生随机整数            | low：最小值；high：最大值；size：数据个数                    |
| random_sample([size])             | 在[0,1）内产生随机数    | size：随机数的shape，可以为元祖或者列表，[2,3]表示2维随机数，维度为（2,3） |
| random([size])                    | 同random_sample([size]) | 同random_sample([size])                                      |
| ranf([size])                      | 同random_sample([size]) | 同random_sample([size])                                      |
| sample([size]))                   | 同random_sample([size]) | 同random_sample([size])                                      |
| choice(a[, size, replace, p])     | 从a中随机选择指定数据   | a：1维数组 size：返回数据形状                                |
| bytes(length)                     | 返回随机位              | length：位的长度                                             |

## numpy.random.rand()

numpy.random.rand(d0,d1,…,dn)

Use .rand to get one or a list of random sample from [0,1).

```python
>>> np.random.rand()
0.18764147324357994
>>> np.random.rand(2,3)
array([[ 0.50590041,  0.37166178,  0.86995256],
       [ 0.16217236,  0.88082289,  0.98760503]])
```

## numpy.random.randn()

numpy.random.randn(d0,d1,…,dn)

Use .randn to get one or a list of **standard normal distribution** sample.

```python
>>> np.random.randn(2,3)
array([[-0.08735649, -1.20363118, -0.67110949],
       [ 0.00484652,  0.22180857,  0.25103274]])
```

## numpy.random.randint()

numpy.random.randint(low, high=None, size=None, dtype=’l’)

```python
>>> np.random.randint(0,10,size=(2,3))
array([[1, 2, 5],
       [8, 8, 9]])
```

## numpy.random.choice()

numpy.random.choice(a, size=None, replace=True, p=None)

> a : 1-D array-like or int If an ndarray, a random sample is generated from its elements.  If an int, the random sample is generated as if a was np.arange(n) 
>
> size : int or tuple of ints, optional  replace : boolean, optional Whether the sample is with or without replacement
>
> p : 1-D array-like, optional The probabilities associated with each entry in a. If not given the sample assumes a uniform distribution over all entries in a. 

```python
# 随机取一个小于指定数的随机数
>>> np.random.choice(10)
2
# 随机取n个小于指定数的随机数序列
>>> np.random.choice(10,10)
array([3, 2, 0, 6, 0, 0, 2, 2, 4, 5])
# 随机取形状为(m, n)的小于指定数的随机数矩阵
>>> np.random.choice(10,(2,3))
array([[2, 2, 3],
       [0, 4, 1]])
# 从序列中随机抽取n个元素，组成一个新数列
>>> classes = ['plane', 'car', 'bird', 'cat', 'deer', 'dog', 'frog', 'horse', 'ship', 'truck']
>>> np.random.choice(classes, 2)
array(['deer', 'ship'], dtype='<U5')
# 按照概率分布抽取元素
>>> np.random.choice(range(3), p=[0.05,0.05,0.9])
2
```

.choice(x)的参数x可以是Integer也可以是一维数组1-D Array。

`np.random.choice(range(3),p=[0.05,0.05,0.9])`表示从range(3)序列中按照p参数指定的概率分布随机选择一个元素返回。

# 随机生成分布

各种分布随机数的API：

| 函数名称                                     | 函数功能                                                     | 参数说明 |
| -------------------------------------------- | ------------------------------------------------------------ | -------- |
| beta(a, b[, size])                           | 贝塔分布样本，在 [0, 1]内。                                  |          |
| binomial(n, p[, size])                       | 二项分布的样本。                                             |          |
| chisquare(df[, size])                        | 卡方分布样本。                                               |          |
| dirichlet(alpha[, size])                     | 狄利克雷分布样本。                                           |          |
| exponential([scale, size])                   | 指数分布                                                     |          |
| f(dfnum, dfden[, size])                      | F分布样本。                                                  |          |
| gamma(shape[, scale, size])                  | 伽马分布                                                     |          |
| geometric(p[, size])                         | 几何分布                                                     |          |
| gumbel([loc, scale, size])                   | 耿贝尔分布。                                                 |          |
| hypergeometric(ngood, nbad, nsample[, size]) | 超几何分布样本。                                             |          |
| laplace([loc, scale, size])                  | 拉普拉斯或双指数分布样本                                     |          |
| logistic([loc, scale, size])                 | Logistic分布样本                                             |          |
| lognormal([mean, sigma, size])               | 对数正态分布                                                 |          |
| logseries(p[, size])                         | 对数级数分布。                                               |          |
| multinomial(n, pvals[, size])                | 多项分布                                                     |          |
| multivariate_normal(mean, cov[, size])       | 多元正态分布。                                               |          |
| negative_binomial(n, p[, size])              | 负二项分布                                                   |          |
| noncentral_chisquare(df, nonc[, size])       | 非中心卡方分布                                               |          |
| noncentral_f(dfnum, dfden, nonc[, size])     | 非中心F分布                                                  |          |
| normal([loc, scale, size])                   | 正态(高斯)分布                                               |          |
| pareto(a[, size])                            | 帕累托（Lomax）分布                                          |          |
| poisson([lam, size])                         | 泊松分布                                                     |          |
| power(a[, size])                             | Draws samples in [0, 1] from a power distribution with positive exponent a - 1. |          |
| rayleigh([scale, size])                      | Rayleigh 分布                                                |          |
| standard_cauchy([size])                      | 标准柯西分布                                                 |          |
| standard_exponential([size])                 | 标准的指数分布                                               |          |
| standard_gamma(shape[, size])                | 标准伽马分布                                                 |          |
| standard_normal([size])                      | 标准正态分布 (mean=0, stdev=1).                              |          |
| standard_t(df[, size])                       | Standard Student’s t distribution with df degrees of freedom. |          |
| triangular(left, mode, right[, size])        | 三角形分布                                                   |          |
| uniform([low, high, size])                   | 均匀分布                                                     |          |
| vonmises(mu, kappa[, size])                  | von Mises分布                                                |          |
| wald(mean, scale[, size])                    | 瓦尔德（逆高斯）分布                                         |          |
| weibull(a[, size])                           | Weibull 分布                                                 |          |
| zipf(a[, size])                              | 齐普夫分布                                                   |          |

## numpy.random.normal() 

Numpy的随机分布函数中最常用的应该就是normal函数，

```python
import numpy as np
import matplotlib.pyplot as plt

mu = 1  #期望为1
sigma = 3  #标准差为3
num = 10000  #个数为10000

rand_data = np.random.normal(mu, sigma, num)
count, bins, ignored = plt.hist(rand_data, 30, normed=True)
plt.plot(bins, 1/(sigma * np.sqrt(2 * np.pi)) *np.exp( - (bins - mu)**2 / (2 * sigma**2)), linewidth=2, color='r')
plt.show()
```

![](Numpy-06-random\normal.png)

## numpy.random.rand()

定义如下，numpy.random.rand(d0, d1, ..., dn)

> Random values in a given shape.
>
> Create an array of the given shape and populate it with random samples from a uniform distribution over [0, 1).
>
> Parameters:	
> d0, d1, …, dn : int, optional
> The dimensions of the returned array, should all be positive. If no argument is given a single Python float is returned.
>
> Returns:	
> out : ndarray, shape (d0, d1, ..., dn)
> Random values.

按指定形状生成介于[0, 1)的均匀随机分布。

## numpy.random.uniform()

numpy.random.uniform(low=0.0, high=1.0, size=None)

> Draw samples from a uniform distribution.
>
> Samples are uniformly distributed over the half-open interval [low, high) (includes low, but excludes high). In other words, any value within the given interval is equally likely to be drawn by uniform.
>
> Parameters:	
> low : float or array_like of floats, optional
> Lower boundary of the output interval. All values generated will be greater than or equal to low. The default value is 0.
>
> high : float or array_like of floats
> Upper boundary of the output interval. All values generated will be less than high. The default value is 1.0.
>
> size : int or tuple of ints, optional
> Output shape. If the given shape is, e.g., (m, n, k), then m * n * k samples are drawn. If size is None (default), a single value is returned if low and high are both scalars. Otherwise, np.broadcast(low, high).size samples are drawn.
>
> Returns:	
> out : ndarray or scalar
> Drawn samples from the parameterized uniform distribution.

按指定形状生成介于[low, high)的均匀随机分布。

# 参考

https://www.cnblogs.com/xk-bench/p/9079533.html

