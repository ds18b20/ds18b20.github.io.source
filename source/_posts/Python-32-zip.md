---
title: Python-32-zip
date: 2018-03-31 15:33:36
categories: Python
tags:
- zip
---

# zip

```python
a = (0, 1, 2)
b = (3, 4, 5)
y = list(zip(a, b))
print(y)
```

得到，

```
[(0, 3), (1, 4), (2, 5)]
```

a和b长度不同时，按较短的序列长度来组合。

```python
>>> list(zip(a,b))
[(0, 3), (1, 4), (2, 5)]
>>> list(zip(b,a))
[(3, 0), (4, 1), (5, 2)]
```

# unzip

```python
x = list(zip(*y))
print(x)
```

得到，

```
[(0, 1, 2), (3, 4, 5)]
```

可以看到这就是上面输入的a和b。