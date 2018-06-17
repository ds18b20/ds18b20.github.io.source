---
title: Matplotlib-01-Basic
date: 2017-11-08 23:42:49
categories: Python
tags:
- Python
- Matplotlib
- Basic
---

# Basic of Matplotlib

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(0,6,0.1)

y1 = np.sin(x)
y2 = np.cos(x)

# draw graph
plt.plot(x, y1, label="sin")
plt.plot(x, y2, linestyle="--", label="cos")

plt.xlabel("x")
plt.ylabel("y")
plt.title("sin & cos")
plt.legend()

plt.show()
```

Result:

![01-plot](Matplotlib-01-Basic/01-plot.png)