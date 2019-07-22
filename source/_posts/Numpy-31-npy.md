---
title: Numpy-31-npy
date: 2018-12-17 22:28:28
categories: Python
tags:
- Numpy
- npy

---

# numpy save load

This example shows how to save and load data.

```python
import numpy as np

d1={'key1':[5,10], 'key2':[50,100]}
np.save("d1.npy", d1)
d2=np.load("d1.npy")
print(d1.get('key1'))
print(d2.item().get('key2'))
```

# Reference

https://stackoverflow.com/questions/40219946/python-save-dictionaries-through-numpy-save

