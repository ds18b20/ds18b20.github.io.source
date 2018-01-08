---
title: Numpy-09-ArraySlice
date: 2017-11-30 23:44:12
categories: Python
tags:
- Numpy
- Python
- nditer
---

# Numpy Array Slice

## Slice Numpy Array

```python
>>> seq[:]                # [seq[0],   seq[1],          ..., seq[-1]    ]
>>> seq[low:]             # [seq[low], seq[low+1],      ..., seq[-1]    ]
>>> seq[:high]            # [seq[0],   seq[1],          ..., seq[high-1]]
>>> seq[low:high]         # [seq[low], seq[low+1],      ..., seq[high-1]]
>>> seq[::stride]         # [seq[0],   seq[stride],     ..., seq[-1]    ]
>>> seq[low::stride]      # [seq[low], seq[low+stride], ..., seq[-1]    ]
>>> seq[:high:stride]     # [seq[0],   seq[stride],     ..., seq[high-1]]
>>> seq[low:high:stride]  # [seq[low], seq[low+stride], ..., seq[high-1]]
```

Examples:

```python
>>> import numpy as np
>>> a=np.array([[1,2,3],[4,5,6],[7,8,9]])
>>> a
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])
>>> a[0:2]
array([[1, 2, 3],
       [4, 5, 6]])
>>> a[::2]
array([[1, 2, 3],
       [7, 8, 9]])
```

If `stride` is negative, we will use counting down order:

```python
>>> seq[::-stride]        # [seq[-1],   seq[-1-stride],   ..., seq[0]    ]
>>> seq[high::-stride]    # [seq[high], seq[high-stride], ..., seq[0]    ]
>>> seq[:low:-stride]     # [seq[-1],   seq[-1-stride],   ..., seq[low+1]]
>>> seq[high:low:-stride] # [seq[high], seq[high-stride], ..., seq[low+1]]
```

Examples:

```python
>>> a[3::-1]
array([[7, 8, 9],
       [4, 5, 6],
       [1, 2, 3]])
```

## Use Comma

`[:, 0]` means `[first_row:last_row, column_0]`

```python
>>> a
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])

>>> a[::2,2]
array([3, 9])
```

## Slice by 2 arrays

```python
>>> import numpy as np
>>> a=np.arange(9).reshape(3,3)
>>> a
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])

>>> x=np.array([0,1,2])
>>> y=np.array([2,1,0])

>>> a[x,y]
array([2, 4, 6])
```

![SlicebyArray](Numpy-09-ArraySlice/SlicebyArray.JPG)

