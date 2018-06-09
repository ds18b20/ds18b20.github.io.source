---
title: Matplotlib-04-axes
date: 2018-03-22 19:35:11
categories: Python
tags:
- Matplotlib
- Python
- axes
- figure
---

# Matplotlib axes

从英文本意的角度上理解axes就是axis的复数形式，但是在matplotlib中axes并不是多个轴的意义。

讲到axes就离不开另一个概念：figure。figure可以理解为画布或者进程，定义一个figure之后就可以显示一个空白的窗口。即使没有特意生成figure，pyplot也会帮你创建一个（figure 1）。

有了画布，继续画图时就需要决定在哪里，画多大的图。这一部分的设置就靠axes来表示，因为一个axes就是一个不包含坐标轴的画图区域。位置和尺寸均是用占figure整体尺寸的百分比来表示的，其取值为0-1之间的浮点数。

![](Matplotlib-04-axes\fig.png)

一个官方的例子如下，

```python
import matplotlib.pyplot as plt
import numpy as np

# create some data to use for the plot
dt = 0.001
t = np.arange(0.0, 10.0, dt)
r = np.exp(-t[:1000]/0.05)               # impulse response
x = np.random.randn(len(t))
s = np.convolve(x, r)[:len(x)]*dt  # colored noise


# the main axes is subplot(111) by default
plt.plot(t, s)
plt.axis([0, 1, 1.1*np.amin(s), 2*np.amax(s)])
plt.xlabel('time (s)')
plt.ylabel('current (nA)')
plt.title('Gaussian colored noise')

# this is an inset axes over the main axes
a = plt.axes([.65, .6, .2, .2], facecolor='y')
n, bins, patches = plt.hist(s, 400, normed=1)
plt.title('Probability')
plt.xticks([])
plt.yticks([])

# this is another inset axes over the main axes
a = plt.axes([0.2, 0.6, .2, .2], facecolor='y')
plt.plot(t[:len(r)], r)
plt.title('Impulse response')
plt.xlim(0, 0.2)
plt.xticks([])
plt.yticks([])

plt.show()
```

结果为，

![](Matplotlib-04-axes\axes.png)

# 隐藏坐标轴

```python
fig=plt.figure(frameon=False)
fig.set_size_inches(1, 1)

ax = plt.Axes(fig, [0., 0., 1., 1.])
ax.set_axis_off()
fig.add_axes(ax)

I = mpimg.imread(u"sample.png")
ax.imshow(I)
fig.savefig(u"sample.jpg", dpi=32)
```

# 参考

pyplot的基础概念

http://blog.sina.com.cn/s/blog_6bba71920102wj2e.html

pylab_examples example code: axes_demo.py

https://matplotlib.org/examples/pylab_examples/axes_demo.html