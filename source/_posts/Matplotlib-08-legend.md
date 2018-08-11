---
title: Matplotlib-08-legend
date: 2018-07-15 23:49:18
categories: Python
tags:
- Matplotlib
- Python
- legend

---

# legend

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 2, 100)

plt.plot(x, x, label='linear')
plt.plot(x, x**2, label='quadratic')
plt.plot(x, x**3, label='cubic')

plt.xlabel('x label')
plt.ylabel('y label')

plt.title("Simple Plot")

plt.legend()

plt.show()
```

得到图像，

![](Matplotlib-08-legend\legend.png)