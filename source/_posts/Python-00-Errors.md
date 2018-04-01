---
title: Python-00-Errors
date: 2018-01-24 23:06:10
categories: Python
tags:
- Python
- errors
---

# Function-takes 1 positional argument but 2 were given

- 检查参数个数是否和定义一致
- 另外，特别要注意一下class里的函数定义时，有没有忘记self参数


# List-list assignment index out of range

```python
# List
delta = 0.01
grad = []

for i in range(3):
    grad[i] = i + delta
print(grad)
```

这时就会抛出`list assignment index out of range`错误。

因为grad是一个空list，第0个元素也不存在，故向grad[0]写数据就会报错。

解决办法：

1. 开始位置初始化List的长度，如grad = [0, 0, 0]
2. 使用list的append方法，如grad.append(i + delta)

