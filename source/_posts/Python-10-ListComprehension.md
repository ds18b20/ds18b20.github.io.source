---
title: Python-10-ListComprehension
date: 2018-02-07 23:30:12
categories: Python
tags:
- Python
- List Comprehension

---

#Python list comprehension

列表生成(list comprehension)是一种Python内置的生成式，使用它可以快速方便地创建新的list。

最简单的创建list的方式，你可能想到：

```python
# 前闭后开
>>> list(range(1, 11))

>>> L = []
>>> for x in range(1, 11):
...    L.append(x * x)
```

使用列表生成式时，可以

```python
>>> [x*x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

for之后可以加上if判断，例如这就可以只做偶数的平方

```python
>>> [x*x for x in range(1, 11) if x%2 == 0]
[4, 16, 36, 64, 100]
```

还可以使用双层循环，实现全排列

```python
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```

