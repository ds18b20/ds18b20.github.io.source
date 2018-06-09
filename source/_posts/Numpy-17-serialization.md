---
title: Numpy-17-serialization
date: 2018-04-22 00:57:32
categories: Python
tags:
- Numpy
- Python
- serialization
- json
---

# Numpy的序列化

Numpy的ndarray对象无法直接使用json直接序列化，需要先转化成有同样层次结构的List对象，然后再序列化为json文件。

转换成List对象可以使用Numpy自带的.to_list()方法。

```python
import numpy as np
import codecs, json 

a = np.arange(10).reshape(2,5) # a 2 by 5 array
b = a.tolist() # nested lists with same data, indices
file_path = "/path.json" ## your path variable
json.dump(b, codecs.open(file_path, 'w', encoding='utf-8'), separators=(',', ':'), sort_keys=True, indent=4) ### this saves the array in .json format
```

In order to unjonsify use

```python
obj_text = codecs.open(file_path, 'r', encoding='utf-8').read()
b_new = json.loads(obj_text)
a_new = np.array(b_new)
```

# 参考

NumPy array is not JSON serializable

https://stackoverflow.com/questions/26646362/numpy-array-is-not-json-serializable