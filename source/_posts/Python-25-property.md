---
title: Python-25-property
date: 2018-02-20 21:11:23
categories: Python
tags:
- Python
- @property
---

# @property

```python
class Student(object):
    pass
a = Student()
a.name = 'Tom'
```

像这样直接操作属性很方便，但是这样写出的代码可靠性不高。因为内部属性更容易被误操作，而且没有实现参数检查。

比较标准的做法是，在class内创建函数，用于读写内部属性。

```python
class Student(object):
    def get_score(self):
         return self._score
    def set_score(self, value):
        if isinstance(value, int):
            if value >= 0 and value<=100:
                self._score = value
            else:
                raise ValueError('score must between 0 ~ 100!')
        else:
            raise ValueError('score must be an integer!')

s = Student()
s.set_score(95)
print(s.get_score())
```

这样虽然没有问题，但是调用函数毕竟没有**直接操作属性**方便。

这时可以使用@property装饰器来用直接操作的形式实现相同的结果。

```python
class Student(object):
    @property
    def score(self):
         return self._score
    @score.setter
    def score(self, value):
        if isinstance(value, int):
            if value >= 0 and value<=100:
                self._score = value
            else:
                raise ValueError('score must between 0 ~ 100!')
        else:
            raise ValueError('score must be an integer!')

s = Student()
s.score = 95
print(s.score)
```

形式上很简单地直接操作了属性，但是实际上在内部也实现了参数检查。

# 只读属性

通过不定义@***.setter来实现只读属性。

```python
class Student(object):
    @property
    def love(self):
         return self._love

s = Student()
# 这句会报错
s.love = 95
```

上述代码中的love是只读属性，故不可以修改。

# 例子

```python
# Q
class Screen(object):
    @property
    def width(self):
        return self._width
    @width.setter
    def width(self, value):
        if isinstance(value, int):
                self._width = value
        else:
            raise ValueError('width must be an integer!')

    @property
    def height(self):
        return self._height
    @height.setter
    def height(self, value):
        if isinstance(value, int):
                self._height = value
        else:
            raise ValueError('height must be an integer!')
    
    @property
    def resolution(self):
        return self._width * self._height

# 测试:
s = Screen()
s.width = 1024
s.height = 768
print('resolution =', s.resolution)
if s.resolution == 786432:
    print('测试通过!')
else:
    print('测试失败!')
```

