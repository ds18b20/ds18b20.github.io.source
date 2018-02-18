---
title: Python-18-ListDel
date: 2018-02-16 22:00:17
categories: Python
tags:
- Python
- List
- Delete
---

# List.pop(pos)

弹出序列中pos位置的元素，并**返回**此元素。

```python
>>> a = [0,1,2,3,4,5]
>>> x = a.pop(1)
>>> a
[0, 2, 3, 4, 5]
>>> x
1
```

# List.remove(x)

从List中删除第一个x

```python
>>> a = [0,1,2,3,4,5]
>>> a.remove(1)
>>> a
[0, 2, 3, 4, 5]
```

# del List[pos]

从List中删除pos位置的元素

```python
>>> a = [0,1,2,3,4,5]
>>> del a[1]
>>> a
[0, 2, 3, 4, 5]
```

