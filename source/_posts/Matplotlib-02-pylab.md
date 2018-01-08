---
title: Matplotlib-02-pylab
date: 2017-11-10 20:55:31
categories: Python
tags:
- Matplotlib
- pylab
- pyplot

---

# pylab or pyplot

## pylab

[Link](https://matplotlib.org/faq/usage_faq.html)

`pylab is a convenience module that bulk imports matplotlib.pyplot (for plotting) and numpy (for mathematics and working with arrays) in a single name space. Although many examples use pylab, it is no longer recommended.`

```python
import matplotlib.pylab as plt
```

## pyplot

```python
import matplotlib.pyplot as plt
import numpy as np
```

## comment

Try to import `plot` and `numpy` separately. Follow the rule of "import explicitly".