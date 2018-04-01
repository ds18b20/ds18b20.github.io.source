---
title: Numpy-14-mgrid
date: 2018-03-31 15:16:16
categories: Python
tags:
- Numpy
- Python
- mgrid
---

# generate mesh grid array

```python
import numpy as np

x,y = np.mgrid[0:5, 0:3]
print(x)
print(y)
```

return:

```
[[0 0 0]
 [1 1 1]
 [2 2 2]
 [3 3 3]
 [4 4 4]]
[[0 1 2]
 [0 1 2]
 [0 1 2]
 [0 1 2]
 [0 1 2]]
```

# usage

```python
%matplotlib inline
import numpy as np
import matplotlib.pyplot as plt

X,Y  = np.mgrid[-2:2:0.5, -2:2:0.5]

plt.figure(figsize=(5,5),dpi=80)
plt.quiver(X,Y)
plt.show()
```

you can get,

![](Numpy-14-mgrid\mgrid.png)

```python
# X:
[[-2.  -2.  -2.  -2.  -2.  -2.  -2.  -2. ]
 [-1.5 -1.5 -1.5 -1.5 -1.5 -1.5 -1.5 -1.5]
 [-1.  -1.  -1.  -1.  -1.  -1.  -1.  -1. ]
 [-0.5 -0.5 -0.5 -0.5 -0.5 -0.5 -0.5 -0.5]
 [ 0.   0.   0.   0.   0.   0.   0.   0. ]
 [ 0.5  0.5  0.5  0.5  0.5  0.5  0.5  0.5]
 [ 1.   1.   1.   1.   1.   1.   1.   1. ]
 [ 1.5  1.5  1.5  1.5  1.5  1.5  1.5  1.5]]
# Y:
[[-2.  -1.5 -1.  -0.5  0.   0.5  1.   1.5]
 [-2.  -1.5 -1.  -0.5  0.   0.5  1.   1.5]
 [-2.  -1.5 -1.  -0.5  0.   0.5  1.   1.5]
 [-2.  -1.5 -1.  -0.5  0.   0.5  1.   1.5]
 [-2.  -1.5 -1.  -0.5  0.   0.5  1.   1.5]
 [-2.  -1.5 -1.  -0.5  0.   0.5  1.   1.5]
 [-2.  -1.5 -1.  -0.5  0.   0.5  1.   1.5]
 [-2.  -1.5 -1.  -0.5  0.   0.5  1.   1.5]]
```

