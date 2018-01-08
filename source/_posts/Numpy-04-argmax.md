---
title: Numpy-04-argmax
date: 2017-11-14 22:32:24
categories: Python
tags:
- Numpy
- Python
- argmax
---

# np.argmax

```python
>>> a
array([1, 5, 3, 7])
>>> np.argmax(a)
3

>>> b
array([[1, 5, 5, 2],
       [6, 6, 2, 8],
       [3, 7, 9, 1]])
>>> np.argmax(b)
10

# Vertical comparation
# ----
# ----
# ----
>>> np.argmax(b, axis=0)
array([1, 2, 2, 1], dtype=int64)

# Horizonal comparation
# ||||
# ||||
# ||||
>>> np.argmax(b, axis=1)
array([1, 3, 2], dtype=int64)
```

