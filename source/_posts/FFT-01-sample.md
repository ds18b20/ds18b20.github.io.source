---
title: FFT-01-sample
date: 2018-04-11 22:43:21
categories: FFT
tags:
- FFT
- sample
---

# Sample code for FFT

```python
%matplotlib inline
import matplotlib.pyplot as plt
import numpy as np

class iFFT(object):
    def __init__(self, ori_array, sampling_period):
        self.__ori_array = ori_array
        self.__count = len(self.__ori_array)
        self.__sampling_period = sampling_period
        self.__interval = self.__sampling_period / self.__count

    def get_ori_xy(self):
        x = np.arange(0, self.__sampling_period, self.__interval)
        y = self.__ori_array

        return x, y

    def get_fft_xy(self):
        x = np.arange(self.__count / 2) / self.__sampling_period
        ft = np.fft.fft(self.__ori_array)
        y = abs(ft[range(int(self.__count / 2))] / self.__count)

        return x, y

period = 5
interval = 0.005
time = np.arange(0, period, interval)
array = np.sin(2 * np.pi * 10 * time) + 0.5 * np.sin(2 * np.pi * 30 * time)
sample = iFFT(array, period)
x0, y0 = sample.get_ori_xy()
x1, y1 = sample.get_fft_xy()

plt.subplot(2, 1, 1)
plt.plot(x0, y0, 'black')
plt.xlabel('Time'), plt.ylabel('Amplitude')

plt.subplot(2,1,2)
plt.plot(x1, y1, 'red')
plt.xlabel('Freq (Hz)'), plt.ylabel('Amp. Spectrum')
plt.show()
```

the output,

![](FFT-01-sample\fft.png)