---
title: Python-20-PartialFunction
date: 2018-02-18 23:35:24
categories: Python
tags:
- Python
- Partial Function
---

# 偏函数的应用场合

利用已有函数，固定一部分参数，得到另一个函数。

```python
import functools

int2 = functools.partial(int, base=2)
int2('1000000')
```

输出为64

这样就以函数int()得到了一个新函数，名为int2()。

注意，调用int2()时，可以更改默认你参数base为其他值。

```python
>>> int2('1000000', base=10)
1000000
```

# 原理

其实是把已有函数后的参数，以\*args和\*\*kw的方式传入。包含参数名时，以\*\*kw的方式传入。不包含参数名时，以\*args的方式传入。

```python
max2 = functools.partial(max, 10)
max2(5, 6, 7)
```

相当于，

```python
args = (10, 5, 6, 7)
max(*args)
```

输出为10

