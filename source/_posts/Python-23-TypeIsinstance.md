---
title: Python-23-TypeIsinstance
date: 2018-02-21 21:28:31
categories: Python
tags:
- Python
- type
- isinstance
---

# type

type()函数用来判断对象的类型。基本类型/指向函数或者类的对象都可以使用type函数。

```python
# 基本类型
>>> type(12)
<class 'int'>
>>> type('a')
<class 'str'>

# 函数或者类
>>> def foo():
...     pass
...
>>> type(foo)
<class 'function'>
```

实际上type(a)返回的是a对应的class类型。

```python
>>> type('abc')==str
True
```

像上面那样判断基本类型，可以直接用内制的class名。但是要判断一个对象是否是函数应该怎么写呢？ `type(foo) == ?`

```python
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```

通过引入types模块来判断。

使用type()有一定的局限性，即无法追溯到类型的父类。这时候可以使用isinstance函数

# isinstance

isinstance不仅可以判断对象是不是其类本身，还可以判断其是否属于其父类下。

所以，isinstance也可以用来判断基本类型，覆盖了type的部分功能。

```python
# 判断父类
# object -> Animal -> Dog -> Husky
>>> a = Animal()
>>> d = Dog()
>>> h = Husky()
>>> isinstance(h, Husky)
True
>>> isinstance(h, Dog)
True

# 判断基本类型
>>> isinstance(123, int)
True
>>> isinstance(b'a', bytes)
True

# 还可以判断多个
>>> isinstance([1, 2, 3], (list, tuple))
True
```

# others

## dir(object)

返回object的所有属性和方法。

注意class和instance的返回结果可能不一样。于init函数的存在，instance的属性可能会多一些。

## hasattr(object, attribute)

用来判断object是否包含属性attribute。另外还有setattr()函数和getattr()函数。