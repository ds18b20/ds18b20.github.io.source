---
title: Pandas-03-reindex
date: 2018-10-07 22:34:19
categories: Pandas
tags:
- reindex

---

# pandas.reindex

reindex方法只是根据参数来更改原数组的排列顺序，而不会修改原有数据。如果参数中指定的名称在原数组中不存在，则用NaN代替。

```python
import pandas as pd
import numpy as np
obj = pd.Series(range(4), index=['d', 'b', 'a', 'c'])
print obj
```

输出，

```
d    0
b    1
a    2
c    3
dtype: int64 
```

使用reindex修改排列顺序后，

```python
obj.reindex(['a', 'b', 'c', 'd', 'e'])
```

输出，

```
a    2.0
b    1.0
c    3.0
d    0.0
e    NaN
dtype: float64
```

同样地，DataFrame也有reindex方法来修改行或者列的顺序。

```python
frame = pd.DataFrame(np.arange(9).reshape((3, 3)), index=['a', 'c', 'd'], columns=['c1', 'c2', 'c3'])

states = ['c1', 'b2', 'c3']
frame.reindex(columns=states)

frame_na=frame.reindex(index=['a', 'b', 'c', 'd'], method='ffill', columns=states)
```

针对NaN值可以使用fillna方法来填充，

```python
frame_na.fillna(method='ffill',axis=1)
```

# Reference

https://blog.csdn.net/LY_ysys629/article/details/55005201