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

这是因为imshow函数对输入数据的格式和类型有要求：

`matplotlib.pyplot.imshow`(*X*, *cmap=None*, *norm=None*, *aspect=None*, *interpolation=None*, *alpha=None*, *vmin=None*, *vmax=None*, *origin=None*, *extent=None*, *shape=None*, *filternorm=1*, *filterrad=4.0*, *imlim=None*, *resample=None*, *url=None*, ***, *data=None*, **\*kwargs*)

> **X** : array-like or PIL image
>
> The image data. Supported array shapes are:
>
> - (M, N): an image with scalar data. The data is visualized using a colormap.
> - (M, N, 3): an image with RGB values (float or uint8).
> - (M, N, 4): an image with RGBA values (float or uint8), i.e. including transparency.
>
> The first two dimensions (M, N) define the rows and columns of the image.
>
> The RGB(A) values should be in the range [0 .. 1] for floats or [0 .. 255] for integers. Out-of-range values will be clipped to these bounds.

故，作为显示数据的X必须是[0, 1]的float或者[0, 255]的uint8类型。

另外一个int的例子是，当数据类型是默认的int32类型时，显示颜色不正常：

![](Matplotlib-09-imshow\int32.png)

改成uint8类型之后，显示颜色变为正常了：

![](Matplotlib-09-imshow\uint8.png)

变换函数：`numpy_data.astype('uint8')`

# Reference

https://stackoverflow.com/questions/35420749/imshow-inverts-colors-after-using-astypefloat
https://stackoverflow.com/questions/24739769/matplotlib-imshow-plots-different-if-using-colormap-or-rgb-array



