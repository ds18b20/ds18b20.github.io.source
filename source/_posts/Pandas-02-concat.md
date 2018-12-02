---
title: Pandas-02-concat
date: 2018-10-07 22:34:06
categories: Pandas
tags:
- concat

---

# pandas.concat

函数定义如下：

> pandas.concat
>
> `pandas.concat`(*objs*, *axis=0*, *join='outer'*, *join_axes=None*, *ignore_index=False*, *keys=None*, *levels=None*, *names=None*, *verify_integrity=False*, *sort=None*, *copy=True*)[[source\]](http://github.com/pandas-dev/pandas/blob/v0.23.4/pandas/core/reshape/concat.py#L21-L226)
>
> Concatenate pandas objects along a particular axis with optional set logic along the other axes.
>
> Can also add a layer of hierarchical indexing on the concatenation axis, which may be useful if the labels are the same (or overlapping) on the passed axis number.

值得注意的是，要组合concat的数组要先组成一个tuple或者list然后传递给*objs*。

```python
frames = [df1, df2, df3]
result = pd.concat(frames)
```

分开传入时会报错。

## ignore_index

```python
s_3 = pd.concat((s_1, s_2), ignore_index=True)
```

这样可以clear掉原有的index，并填上默认的index。

或者，可以使用reset_index方法来重置index。

```python
result2= result.reset_index()
```

# Reference

https://pandas.pydata.org/pandas-docs/stable/generated/pandas.concat.html#pandas-concat

https://blog.csdn.net/stevenkwong/article/details/52528616

