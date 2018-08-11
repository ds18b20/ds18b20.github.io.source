---
title: Python-09-is
date: 2018-02-06 22:46:54
categories: Python
tags:
- Python
- is
---

# 基础准备

Python对象包括三个基本要素：id，type和value。其中id用来唯一标示一个对象；type用于标示对象的类型，`这里注意一下type()和isinstance()的区别`；而value是对象的值。

`还有，可以参考一下可变量和不可变量。`

# ==和is的区别

简单地讲，`a is b`用来判断a和b是不是指向同一个对象，依据id()来判断。

而`a == b`是判断a和b的value是否相等。比如1 == 1.0，但是id(1) <>id(1.0)

另外，由于数组是可变对象，故两个包含相同数据的数组==，但不是is

# 原理

在Python中is和==都是用来比较两个对象是否“相等”，但是具体使用上二者区别较大。

- is用于比较两个对象是否完全相同，即二者是不是同一个对象。对于Python来讲就是判断二者的id是否相同。
- ==用于比较二者的内容是否相同，而对象的地址可以不相同。对于Python来讲，做==运算时，默认调用对象的\_\_eq\_\_()方法。

下面的例子中采用引用赋值，id不变所以is的结果为True：

```
>>> a = [1, 2, 3]
>>> b = a
>>> b is a 
True
>>> b == a
True
>>> id(a)
4399987272
>>> id(b)
4399987272
```

当进行copy赋值时，id不同所以is的结果也变为False：

```
>>> a = [1, 2, 3]
>>> b = a.copy()
>>> b is a
False
>>> b == a
True
>>> id(a)
4399987592
>>> id(b)
4399987464
```

------

## 使用区别

判断内容是否相同时，当让可以直接使用==运算符，这也是最常见的，但是当相比之下is的运算速度更快。这是因为is不能重载，不像==运算需要调用对象内部的eq方法，这就避免了调用函数带来的开销。
综上，使用is可以提高执行速度，比如is最常用在判断是不是None：
`a is None`
否逻辑为：
`a is not None`

------

## 注意

命令行下Python会对较小的整数对象(范围：[-5, 256])进行缓存，下次调用时直接从混存中获取，此时is和==的结果都是True：

```
>>> a=[1,2,3]
>>> b=[1,2,3]
>>> a==b
True
>>> a is b
False

>>> x=1
>>> y=1
>>> x==y
True
>>> x is y
True
```

当整数当取值超过上述范围时，缓存不再有效：

```
>>> m=257
>>> n=257
>>> m==n
True
>>> m is n
False
```

另外，在Pycharm中运行或者保存到文件再运行的话，isye不成立，因为解释器进行了一部分优化。

# 总结

1、is 比较两个对象的 id 值是否相等，是否指向同一个内存地址；
2、== 比较的是两个对象的内容是否相等，值是否相等；
3、小整数对象[-5,256]在全局解释器范围内被放入缓存供重复使用；
4、is 运算符比 == 效率高，在变量和None进行比较时，应该使用 is。

# 参考

[Python 中的比较：is 与 ==][1]

[How is __eq__ handled in Python and in what order?][2]

[1]: https://www.cnblogs.com/kiko0o0/p/8135184.html
[2]: https://stackoverflow.com/questions/3588776/how-is-eq-handled-in-python-and-in-what-order

