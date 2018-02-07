---
title: Python-11-IteratorIteration
date: 2018-02-07 23:40:11
categories: Python
tags:
- Python
- List Comprehension
---

# Iterator

可直接作用于for循环的数据类型有以下两类：

一类是集合数据类型，如list,tuple,dict,set,str等

一类是generator，包括简单生成器和带yield的generator function

上述可以直接使用for循环的对象统称为可迭代对象，iterable

使用isinstance() 判断一个对象是否Iterable

```python
>>> from collections import Iterable
>>> isinstance([], Iterable)
```

而generator的特殊性表现在其不仅可用for循环，而且还可被next()函数不断调用返回下一个值。

这种可以被next()函数调用并不断计算返回下一个值的对象称为迭代器，Iterator

使用isinstance() 判断一个对象是否Iterator

```python
>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
```

综上，generator都是Iterator，而list之类的集合数据类型则只是Iterable，而不是Iterator

但是可以使用iter()函数把Iterable变成Iterator。

```python
>>> isinstance(iter([]), Iterator)
True
>>> isinstance(iter('abc'), Iterator)
True
```

具体参见

https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143178254193589df9c612d2449618ea460e7a672a366000#0

# iteration

众所周知，list和tuple可以for循环来迭代(iteration)。Python中只要是可迭代对象就可以通过迭代来遍历，比如dict：

```python
d = {'a':1, 'b':2, 'c':3}
for i in d:
	print(i)
```

此时的输出结果很可能不是想象中的顺序，因为dict并不是按照list的方式顺序存储。

默认情况下，dict的迭代输出key。

若要迭代value，可以`for i in d.values()`

如果同时迭代key和value可以用`for k, v in d.items()`

另外要判断一个对象是不是可迭代的（Iterable），可以通过：

```python
>>> from collections import Iterable
>>> isinstance('abc',Iterable)
True
>>> isinstance([1,2,3],Iterable)
True
```

最后，如果想对list实现像C语言一样的按下标循环。可以使用Python内置函数enumerate()来实现。

```python
>>> for i, value in enumerate(['a','b','c']):
...     print(i, value)
...
0 a
1 b
2 c
```

