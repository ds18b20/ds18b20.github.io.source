---
title: Python-16-closure
date: 2018-02-16 14:45:54
categories: Python
tags:
- Python
- Function
- Closure
---

本文从基础开始学习一下Python的闭包。

首先是如何用代码实现，然后是闭包的一些属性，最后是原理上如何实现。

# 怎样实现闭包

通俗地讲，在一个函数A内嵌套另一个函数B，嵌入的函数B内包含了外部函数A的局部变量x，即实现了闭包。这闭包的局部变量x在外部函数A返回后，仍继续存在，并且可被内嵌函数B继续调用。

一个例子

```python
def generate_linear_func(a, b):
    print("id(a, b): %X, %X" % (id(a), id(b)))
    def linear(x):
        return a * x + b
    print("id(linear): %X" % id(linear))
    return linear

# 通过函数generate_linear_func(1, 2)返回linear(x)函数
linear_1_2 = generate_linear_func(1, 2)
# 返回到的linear_1_2就是内建linear(x)的一个引用，地址相同
print("id(linear_1_2): %X" % id(linear_1_2))
# 向linear(x)函数传入参数10
print("a*x+b = %d" % linear_1_2(10))
```

输出结果：

```python
id(a, b): 6D576340, 6D576360
id(linear): 6328D90
id(linear_1_2): 6328D90
a*x+b = 12
```

当移除函数generate_linear_func时，

```python
def generate_linear_func(a, b):
    print("id(a, b): %X, %X" % (id(a), id(b)))
    def linear(x):
        return a * x + b
    print("id(linear): %X" % id(linear))
    return linear

linear_1_2 = generate_linear_func(1, 2)
# 从内存中移除generate_linear_func
del generate_linear_func
print("a*x+b = %d" % linear_1_2(2))
```

仍然可以顺利调用linear_1_2()得到包含有a和b的返回结果。说明函数generate_linear_func(a, b)的局部变量并未被清除，而是以某种方式保存了下来。

# 闭包函数的\__closure__属性和cell对象

闭包的时候，返回函数也是一个对象，这个对象的属性中包含一个名为\__closure__的属性。它定义了一个包含cell对象的tuple，tuple中的元素保存了闭包了的变量值。

例如，

```python
def generate_linear_func(a, b):
    print("id(a, b): %X, %X" % (id(a), id(b)))
    def linear(x):
        return a * x + b
    print("id(linear): %X" % id(linear))
    return linear

linear_1_2 = generate_linear_func(1, 2)
# 元素个数
print("len(cell): %d" % len(linear_1_2.__closure__))
# 迭代显示各元素
for i, j in enumerate(linear_1_2.__closure__):
    print("cell[%d]: %d" % (i, j.cell_contents))
```

如果内建函数并未使用外部函数的局部变量，那么\__closure__ = None

```python
def generate_linear_func(a, b):
    print("id(a, b): %X, %X" % (id(a), id(b)))
    def linear(x):
        return x**2
    print("id(linear): %X" % id(linear))
    return linear

linear_1_2 = generate_linear_func(1, 2)

print(linear_1_2.__closure__)
```

# 闭包的内部原理

关于Python中闭包的具体实现原理，可以参见[Python闭包详解](http://www.cnblogs.com/ChrisChen3121/p/3208119.html) 。文中通过分析Python编译后生成的字节码(code object)来分析其内部运作机制。待学习。。。

# 参考

浅显理解 Python 闭包

https://serholiu.com/python-closures