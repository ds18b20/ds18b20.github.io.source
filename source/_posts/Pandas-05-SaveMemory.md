---
title: Pandas-05-SaveMemory
date: 2018-11-14 21:51:57
categories: Pandas
tags:
- save memory

---

# Pandas save memory

影响内存占用最主要的因素就是数据类型：

The following table shows the subtypes for the most common pandas types:

| memory usage | float   | int   | uint   | datetime   | bool | object |
| ------------ | ------- | ----- | ------ | ---------- | ---- | ------ |
| 1 bytes      |         | int8  | uint8  |            | bool |        |
| 2 bytes      | float16 | int16 | uint16 |            |      |        |
| 4 bytes      | float32 | int32 | uint32 |            |      |        |
| 8 bytes      | float64 | int64 | uint64 | datetime64 |      |        |
| variable     |         |       |        |            |      | object |

An `int8` value uses `1` byte (or `8` bits) to store a value, and can represent `256` values (`2^8`) in binary. This means that we can use this subtype to represent values ranging from `-128` to `127` (including `0`).

https://www.dataquest.io/blog/pandas-big-data/



## psutil

```python
a = pd.read_csv('data\\MNIST\\train.csv')
a.info()

process = psutil.Process(os.getpid())
print(process.memory_info().rss)
```

输出，

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 42000 entries, 0 to 41999
Columns: 785 entries, label to pixel783
dtypes: int64(785)
memory usage: 251.5 MB
330121216
```

加上astype限定数据类型：

```python
a = pd.read_csv('data\\MNIST\\train.csv').astype('uint8')  # 0-255
a.info()

process = psutil.Process(os.getpid())
print(process.memory_info().rss)
```

输出，

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 42000 entries, 0 to 41999
Columns: 785 entries, label to pixel783
dtypes: uint8(785)
memory usage: 31.4 MB
99364864
```

