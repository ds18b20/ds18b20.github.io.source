---
title: Numpy-10-ArrayMerge
date: 2017-12-09 14:13:59
categories: Python
tags:
- Numpy
- Python
- vstack
- row_stack
- hstack
- column_stack
---

# Merge Numpy Array

## Merge by row

numpy.vstack()

numpy.row_stack()

CODE:

```python
>>> import numpy as np

>>> r1=np.array([-5, 0])
>>> r2=np.array([5, 0])

>>> r=np.vstack((p1,p2))
>>> r
array([[-5,  0],
       [ 5,  0]])

>>> r=np.row_stack((p1,p2))
>>> r
array([[-5,  0],
       [ 5,  0]])

```

## Merge by column

numpy.hstack()

```python
>>> temp_array=p.copy()
>>> temp_array
array([[-5,  0],
       [ 5,  0]])
       
>>> c1=temp_array[:,1]
>>> c1
array([0, 0])

>>> c=np.hstack((temp_array,c1.reshape(2,1)))
>>> c
array([[-5,  0,  0],
       [ 5,  0,  0]])
```

numpy.column_stack()

```python
>>> c=np.column_stack((temp_array,c1))
>>> c
array([[-5,  0,  0],
       [ 5,  0,  0]])
```

