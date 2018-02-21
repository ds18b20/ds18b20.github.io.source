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

其实Python的class中还有很多这样有特火速用途的函数，利用这些特殊函数就可以定制属于自己的类。

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

# \_\_getitem\_\_()

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

# \_\_getattr\_\_

使用此方法实现链式调用，进而实现动态化调用。

```python
# 
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
# 
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



# _\_call\_\_

实现直接运行实例，instance()

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