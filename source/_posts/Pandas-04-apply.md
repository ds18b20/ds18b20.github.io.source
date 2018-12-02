---
title: Pandas-04-apply
date: 2018-10-08 14:17:51
categories: Pandas
tags:
- apply

---

# pandas.DataFrame.apply

定义如下，

> `DataFrame.``apply`(*func*, *axis=0*, *broadcast=None*, *raw=False*, *reduce=None*, *result_type=None*, *args=()*, **\*kwds*)[[source\]](http://github.com/pandas-dev/pandas/blob/v0.23.4/pandas/core/frame.py#L5837-L6014)
>
> Apply a function along an axis of the DataFrame.
>
> Objects passed to the function are Series objects whose index is either the DataFrame’s index (`axis=0`) or the DataFrame’s columns (`axis=1`). By default (`result_type=None`), the final return type is inferred from the return type of the applied function. Otherwise, it depends on the result_type argument.

例子，

```python
df = pd.DataFrame([[4, 9],] * 3, columns=['A', 'B'])
df.apply(np.sqrt)
```

## not in-place operation

`.apply()` is not an in-place operation(i.e., it returns a new object rather than modifying the original)

注意，apply不是一个in-place操作函数，即不会修改当前数据集内容。

```python
import numpy as np
import pandas as pd

df = pd.DataFrame([[4, 9],] * 3, columns=['A', 'B'])
df
```

输出，

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>9</td>
    </tr>
  </tbody>
</table>



apply之后的原df数据集：

```python
df.apply(np.sqrt)
df
```

输出，并没有变化：

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>9</td>
    </tr>
  </tbody>
</table>



需要把apply的结果赋值给另一个变量来使用：

```python
x = df.apply(np.sqrt)
x
```

输出，

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>

# Reference

http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html

https://stackoverflow.com/questions/41006982/pandas-apply-function-not-working-consistently-python-3#