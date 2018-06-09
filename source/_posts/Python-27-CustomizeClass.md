---
title: Python-27-CustomizeClass
date: 2018-02-21 21:37:59
categories: Python
tags:
- Python
- Customize Class
---

# class中的特殊函数

在类定义中加入**\_\_slots\_\_属性**可以限制添加到类的属性，加入**\_\_len\_\_()方法**可以让len()函数作用到此类的实例上。比如，

```python
class Student(object):
    def __len__(self):
        return 23
len(Student())
```

其实Python的class中还有很多这样有特殊用途的函数，利用这些特殊函数就可以定制属于自己的类。

# 定制一个自己的类

## \_\_str\_\_()

此方法可以友好地显示实例的信息。

```python
class Student(object):
    def __init__(self, name):
        self._name = name
        
    def __str__(self):
        return 'Student object(name: %s)' % self._name
    
print(Student('John'))
```

输出友好的print显示：

```python
Student object(name: John)
```

但是不加print时，显示<__main__.Student at 0x3dbbfb0>，仍然不是很完美。需要加上一句，

```python
class Student(object):
    def __init__(self, name):
        self._name = name
        
    def __str__(self):
        return 'Student object(name: %s)' % self._name
    __repr__ = __str__
Student('John')
```

输出结果同上。原因在于，对用户显示用的是\_\_str\_\_()方法，而对程序开发者返回使用的是\_\_repr\_\_()方法。通过`__repr__ = __str__`实现对用户的显示。

## \_\_iter\_\_()

要想对一个实例实现类似for * in *的迭代方式，比如迭代list或tuple，就需要在calss内添加\_\_iter\_\_()方法。

其实，可以dir(list)，可以发现list就包含此方法。

```python
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1
    # 实例本身就是迭代的对象，故返回本身
    def __iter__(self):
        return self
    
    def __next__(self):
        self.a, self.b = self.b, self.a + self.b
        if self.a > 10000:
            raise StopIteration()
        return self.a
# 验证
for i in Fib():
    print(i)
```

## \_\_getitem\_\_()

用于按位置取元素。类似于list[n]

```python
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1
        
    # 实例本身就是迭代的对象，故返回本身
    def __getitem__(self, n):
        for i in range(n):
            self.a, self.b = self.b, self.a + self.b
        return self.a
# 验证
s = Fib()
s[5]
```

## \_\_getattr\_\_

使用此方法实现链式调用，进而实现动态化调用。

```python
class Chain(object):

    def __init__(self, path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    __repr__ = __str__

path = 'www.google.com'
c = Chain(path)
c.user.account.net
```

有参数的时候，

```python
class Chain(object):
    def __init__(self, path=''):
        self._path = path
    
    def users(self, name):
        return Chain('%s/users:%s' % (self._path, name))

    def __getattr__(self, attr):
        return Chain('%s/%s' % (self._path, attr))

    def __str__(self):
        return self._path

    __repr__ = __str__

path = 'www.google.com'
c = Chain(path)
# c.users.repos
c.users('michael').repos
```

## _\_call\_\_

实现直接运行实例，instance name()

```python
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)

s = Student('Tom')
s()
```

输出，

```python
My name is Tom.
```

通过callable(ins)来判断对象ins是不是函数。

## \_\_eq\_\_

一般的数值，布尔对象可以直接进行`==`操作，而自定义的对象并不可以。通过\_\_eq\_\_方法可以使`==`操作可以直接作用在自定义对象上。

```python
class Person(object):
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score

    def __eq__(self, other):
        if other is None or not isinstance(other, Person):
            return False
        # return self.name == other.name and self.age == other.age and self.score == other.score
        return self.__dict__ == other.__dict__

    def __ne__(self, other):
        return not self.__eq__(other)


a = Person('Tom', 10, 90)
b = Person('Tom', 10, 90)
# b = Person('Jerry', 10, 90)

print(a == b)
print(dir(Person))
```

上例中`return self.name == other.name and self.age == other.age and self.score == other.score`可以用`self.__dict__ == other.__dict__`更简洁地实现。

输出，

```python
True
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
```

从object继承，自定义一个类Foo的话，默认也会有\_\_eq\_\_方法，但是没有定义其==操作的对象时，只有在两个对象id相同时候(a=b)，才会输出`True`。

```python
class Foo(object):
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score


m = Foo('Tom', 10, 90)
n = Foo('Tom', 10, 90)
# n = m

print(m == n)
print(dir(Foo))
```

得到，

```python
False
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
```

参考，

[Pythonのクラスで__eq__などを汎用的に実装する](https://qiita.com/adatchey/items/1f9acb3e2b66914a435b)

## \_\_add\_\_

```python
class AAA(object):
    def __init__(self, x):
        self.x = x

    def __add__(self, other):
        return self.x + other.x


m = AAA(3)
n = AAA(5)

print(m + n)
```

输出项加后的结果`8`。a + b计算过程中，`__add__方法`定义了前向计算，a中包含该方法，所以可以实现该假发运算。

## \_\_radd\_\_

```python
class X(object):
    def __init__(self, x):
        self.x = x

    def __radd__(self, other):
        # return X(self.x + other.x)
        return self.x + other.x


class Y(object):
    def __init__(self, x):
        self.x = x

    def __radd__(self, other):
        # return X(self.x + other.x)
        return self.x + other.x


a = X(5)
# b = X(10)
b = Y(10)

print(a + b)
```

输出项加后的结果`15`。反向加法，上例a + b中a没有定义`__add__方法`，然后会检查b中有没有定义`__radd__方法`如果有，即可实现加法运算。

参考，

[python核心编程学习笔记-2016-08-15-01-左加法__add__和右加法__radd](https://blog.csdn.net/baidu_21088863/article/details/52214914)

 [python __add__和__radd__](https://blog.csdn.net/u011019726/article/details/77834602) 

# 参考

[流畅的python学习笔记：第十三章：重载运算符__add__,__iadd__,__radd__,__mul__,__rmul__,__neg__,__eq__,__invert__,__pos__](https://www.cnblogs.com/zhanghongfeng/p/7227452.html)

