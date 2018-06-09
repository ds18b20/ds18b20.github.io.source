---
title: Math-05-FFT
date: 2018-05-25 21:32:49
categories: Math
tags:
- FFT
---

# FFT

在频域上处理信号时，最常用的就是FFT(快速傅立叶变换)。时域信号转换成频域信号后可以直观地看到频率分布，并且在频域上更容易进行时域上的滤波、卷积等操作。

## 截取完整周期

```python
# - * -coding: utf-8- * -
import numpy as np
import matplotlib.pyplot as plt

sampling_rate = 8000  # sampling frequency
fft_size = 512  # sample count
t = np.arange(0, 1.0, 1.0 / sampling_rate)  # 8000 pts in [0, 1.0)
x = np.sin(2 * np.pi * 156.25 * t) + 2 * np.sin(2 * np.pi * 234.375 * t)
xs = x[: fft_size]
xf = np.fft.rfft(xs)
freqs = np.linspace(0, sampling_rate / 2, int(fft_size / 2) + 1)
# xfp = 20 * np.log10(np.clip(np.abs(xf), 1e-20, 1e100))
xfp = 2 * np.abs(xf) / fft_size

plt.figure(figsize=(8, 4))

# show Time domain
plt.subplot(211)
plt.plot(t[: fft_size], xs)
plt.xlabel(u"Time/s")
plt.title(u"Wave")

# show frequency domain
plt.subplot(212)
plt.plot(freqs, xfp, 'm')
plt.xlabel(u"Frequency/Hz")
plt.title(u"Spectrum")

plt.subplots_adjust(hspace=0.75)
plt.show()
```

得到

![](Math-05-FFT\FFT_1.png)

`x = np.sin(2 * np.pi * 156.25 * t) + 2 * np.sin(2 * np.pi * 234.375 * t)`生成156.25Hz和234.375Hz的混合信号。

**注意**，此处选择的频率156.25Hz和234.375Hz看似很奇怪，实际上是特意计算得到的。

采样频率8000，采样点512时，采样时长`512点*1/8000s=0.064s`，对应的频率分辨率f0=15.625Hz。上述选择的频率点156.25Hz和234.375Hz分别是f0的10倍和15倍。

`xs = x[: fft_size]`截取长度为fft_size=512长的样本。

`xf = np.fft.rfft(xs)`对样本xs做fft变换。
`freqs = np.linspace(0, sampling_rate / 2, int(fft_size / 2) + 1)`生成频域的频率系列作为横轴。

`xfp = 2 * np.abs(xf) / fft_size`上述得到的xf仍然是复数形式，经过np.abs方法得到xf的绝对值xfs。

`xfp = 2 * np.abs(xf) / fft_size`计算单边振幅，双边都显示时，不用乘2。

## 截取不完整周期

把采样样本改成`x = np.sin(2 * np.pi * 200 * t) + 2 * np.sin(2 * np.pi * 300 * t)`，即200Hz和300Hz的合并信号。同样地fft之后得到，

![](Math-05-FFT\FFT_2.png)

同上方完整截取得到的FFT比较，可以发现主波峰的高度变小，而主峰外的频率值相应的变大了。这意味着本来应该集中在主峰上的能量分散到了周围频率上，这个频率被称为频谱泄露。

> 出现频谱泄漏的原因在于fft_size个取样点无法放下整数个200Hz和300Hz的波形。 

> 频谱泄漏的解释
>
> 我们只能在有限的时间段中对信号进行测量，无法知道在测量范围之外的信号是怎样的。因此只能对测量范围之外的信号进行假设。而傅立叶变换的假设很简单：测量范围之外的信号是所测量到的信号的重复。
>
> 现在考虑512点FFT，从信号中取出的512个数据就是FFT的测量范围，它计算的是这512个数据一直重复的波形的频谱。显然如果512个数据包含整数个周期的话，那么得到的结果就是原始信号的频谱，而如果不是整数周期的话，得到的频谱就是如下波形的频谱，这里假设对50Hz的正弦波进行512点FFT：
>
> ```python
> import numpy as np
> import matplotlib.pyplot as plt
> 
> t = np.arange(0, 1.0, 1.0/8000)
> x = np.sin(2*np.pi*50*t)[:512]
> 
> plt.figure(figsize=(8, 4))
> plt.plot(np.hstack([x,x]))
> plt.xlabel(u"Time/s")
> plt.title(u"Wave")
> 
> pl.show()
> ```
>
> ![](Math-05-FFT\FFT_3.png)

由于截取的样本不是完整的周期的整数倍，可以看到波形在两段的连接处发生了突变。而突变处伴随着非常广泛的频谱，从而导致FFT的结果出现了频谱泄露。另一种解释是，要用傅立叶级数模拟突变处的曲线非常困难，需要多种频谱来配合使用，在FFT结果上表现出来了频谱泄露(spectral leakage)。

## 窗函数

截取不完整周期时，可以通过给截取的片段加窗来优化FFT。

由于截取的周期不完整导致头尾相接处出现了突变，那么可以让头尾部人为地衰减来让连接处变得更加平滑。

常用的窗口函数有Hanning窗函数和Hamming窗函数。

Hanning函数的定义如下：
$$
w(n) = 0.5 - 0.5 \cos\left(\frac{2\pi{n}}{M-1}\right)
\qquad 0 \leq n \leq M-1
$$
Hamming函数的定义如下：
$$
w(n) = 0.54 - 0.46 \cos\left(\frac{2\pi{n}}{M-1}\right)
\qquad 0 \leq n \leq M-1
$$
这两种window的曲线对比如下，

```python
# - * -coding: utf-8- * -
import numpy as np
import matplotlib.pyplot as plt

fft_size = 512
a = np.hanning(fft_size)
b = np.hamming(fft_size)

plt.figure(figsize=(8, 4))
plt.plot(a, 'm', label="Hanning")
plt.plot(b, 'g', label="Hamming")
plt.xlabel(u"Pts")
plt.title(u"Hanning & Hamming window")
plt.legend()

plt.show()
```

![](Math-05-FFT\FFT_4.png)

https://archive.lib.msu.edu/crcmath/math/math/a/a279.htm

上面的200Hz和300Hz的混合信号经过窗函数处理之后，再做FFT如下，

```python
...
x = np.sin(2 * np.pi * 200 * t) + 2 * np.sin(2 * np.pi * 300 * t)
xs = x[: fft_size]

# Multiply by Hanning window
xs = xs * np.hanning(fft_size)
xf = np.fft.rfft(xs)
...
```

得到，

![](Math-05-FFT\FFT_5.png)

可以看到加窗之后FFT的能量更加集中到了200Hz和300Hz附近，频谱泄露得到了较好的改善。

**注意**，Hanning窗本身就有能量衰减，如上图所示，振幅变成了初始信号的一半。所以，要显示正确的振幅(能量)，在FFT的结果上继续乘以2即可。

# iFFT

下面开始用FFT来实现一个低通滤波器。

上述代码稍作修改加入1000Hz的一个noise，

```python
...
noise = np.sin(2 * np.pi * 1000 * t)
x = np.sin(2 * np.pi * 200 * t) + 2 * np.sin(2 * np.pi * 300 * t)
x = x + noise
...
```

得到的曲线如下，

![](Math-05-FFT\FFT_6.png)

尝试对此信号进行低通滤波，包含了加窗--FFT--iFFT--去窗的过程，

```python
# - * -coding: utf-8- * -
import numpy as np
import matplotlib.pyplot as plt

sampling_rate = 8000
fft_size = 512
t = np.arange(0, 1.0, 1.0 / sampling_rate)
noise = np.sin(2 * np.pi * 1000 * t)
x = np.sin(2 * np.pi * 200 * t) + 2 * np.sin(2 * np.pi * 300 * t)
x = x + noise
x_slice = x[: fft_size]
x_slice = x_slice * np.hanning(fft_size)
print('xs shape: ', x_slice.shape)

# fft
x_fft = np.fft.fft(x_slice)
print('xf shape: ', x_fft.shape)

# half spectrum to show
half_freq = sampling_rate / 2
freq_count = int(fft_size / 2) + 1
freqs = np.linspace(0, half_freq, freq_count)
# xfp = 20 * np.log10(np.clip(np.abs(xf), 1e-20, 1e100))

# half spectrum *2
# Hanning window *2
x_fft_power = 2 * 2 * np.abs(x_fft) / fft_size

plt.figure(figsize=(8, 4))

plt.subplot(231)
plt.plot(t[:fft_size], x_slice)
plt.xlabel(u"Time/s")
plt.title(u"Wave")

plt.subplot(234)
plt.plot(freqs, x_fft_power[:freq_count], 'm')
plt.xlabel(u"Frequency/Hz")
plt.title(u"Spectrum")

# low pass filter
low_f = int(500 * fft_size / sampling_rate)
x_fft[low_f:-low_f] = 0
# print('x_fft shape: ', x_fft.shape)
#
# p = 2 * 2 * np.abs(x_fft) / fft_size
#
# plt.subplot(224)
# plt.plot(freqs, p[:freq_count], 'm')
# plt.xlabel(u"Filtered Frequency/Hz")
# plt.title(u"Spectrum")

x_fft_ifft = np.fft.ifft(x_fft)
real = x_fft_ifft.real
print('x_real shape: ', real.shape)

x_fft_ifft_fft = np.fft.fft(x_fft_ifft)
x_fft_ifft_fft_power = 2 * 2 * np.abs(x_fft_ifft_fft) / fft_size
print('x_fft_ifft_fft_power shape: ', x_fft_ifft_fft_power.shape)

plt.subplot(232)
plt.plot(t[: fft_size], real)
plt.xlabel(u"Time/s")
plt.title(u"Filtered Wave")

plt.subplot(235)
plt.plot(freqs, x_fft_ifft_fft_power[:freq_count], 'm')
plt.xlabel(u"Filtered Frequency/Hz")
plt.title(u"Spectrum")

# unwindow
real_unwindow = real / (np.hanning(fft_size) + 1e-10)
real_unwindow_fft = np.fft.fft(real_unwindow)

# double power
real_unwindow_fft_power = 2 * np.abs(real_unwindow_fft) / fft_size
print('real_unwindow_fft shape: ', real_unwindow_fft.shape)

plt.subplot(233)
plt.plot(t[: fft_size], real_unwindow)
plt.xlabel(u"Time/s")
plt.title(u"unwindow Filtered Wave")

plt.subplot(236)
plt.plot(freqs, real_unwindow_fft_power[:freq_count], 'm')
plt.xlabel(u"unwindow Filtered Frequency/Hz")
plt.title(u"Spectrum")

plt.subplots_adjust(hspace=0.75)
plt.show()
```

得到，

![](Math-05-FFT\FFT_7.png)

因为信号已经被window处理过了，当对滤波之后的信号进行还原时，会发现除以window后，信号的前后边沿会发生异常放大。

![](Math-05-FFT\FFT_8.png)

这明显不是先要的结果，原因在于window的两端包含了极小量，除以这小极小量会给结果带来极大的误差。

所以，对于周期信号，可以直接截取完整周期来做FFT。如果没有完整周期的数据，可以先使用上述窗口函数来处理，再做FFT。其实加窗只是在没有其他办法时的一个小trick。

https://www.edaboard.com/showthread.php?168721-How-to-reconstruct-each-frequency-components-by-inverse-FFT

https://stackoverflow.com/questions/40668206/recovering-signal-after-fourier-filter-with-hanning-window

对于大部分采集自自然界的数据，可以认为是非周期信号，可以不加窗直接做FFT处理。

所以对于上面的例子，直接FFT--频域截取--iFFT，得到

![](Math-05-FFT\FFT_9.png)

# 快速卷积

可以使用FFT来加速卷积运算。具体地，

> 我们知道，信号x经过系统h之后的输出y是x和h的卷积，虽然卷积的计算方法很简单，但是当x和h都很长的时候，卷积计算是非常耗费时间的。因此对于比较长的系统h，需要找到比直接计算卷积更快的方法。
>
> 信号系统理论中有这样一个规律：时域的卷积等于频域的乘积，因此要计算时域的卷积，可以将时域信号转换为频域信号，进行乘积运算之后再将结果转换为时域信号，实现快速卷积。

```
# -*- coding: utf-8 -*-
import numpy as np

def fft_convolve(a,b):
    n = len(a)+len(b)-1
    N = 2**(int(np.log2(n))+1)
    A = np.fft.fft(a, N)
    B = np.fft.fft(b, N)
    return np.fft.ifft(A*B)[:n]

if __name__ == "__main__":
    a = np.random.rand(128)
    b = np.random.rand(128)
    c = np.convolve(a,b)

    print np.sum(np.abs(c - fft_convolve(a,b)))
```

速度测试如下，

```
>>> import timeit
>>> setup="""import numpy as np
a=np.random.rand(10000)
b=np.random.rand(10000)
from spectrum_fft_convolve import fft_convolve"""
>>> timeit.timeit("np.convolve(a,b)",setup, number=10)
1.852900578146091
>>> timeit.timeit("fft_convolve(a,b)",setup, number=10)
0.19475575806416145
```

参考链接：https://wizardforcel.gitbooks.io/hyry-studio-scipy/content/20.html

