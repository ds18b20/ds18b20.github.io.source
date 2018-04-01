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