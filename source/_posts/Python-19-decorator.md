---
title: Python-19-decorator
date: 2018-02-18 20:42:22
categories: Python
tags:
- Python
- Decorator
---

# 基本

对于现有函数，如果需要增加某些功能，但又不想对原有逻辑做更改。这时候就可以用装饰器(decorator)来实现。

其实现思路为把既有函数打包到一个新函数内(闭包)，在新函数内新增某些功能，然后通过调用新函数来执行员函数+新增功能。

即使这样，也需要对代码做大量修改，如用新函数替换原函数。在Python中可以使用语法糖@来简单实现，把@加到员函数的定义之前即可。

```python
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper

@log
def foo():
    print('Hello')

@log
def bar():
    print('World')
    
foo()
bar()
```

本质上是

```
x = log(foo)
x()
```

`@functools.wraps(func)`的目的是，加上@log后函数foo的属性就变成wrapper的属性了，通过wraps函数来让foo保持自身属性。

# 带参数的装饰器

log也可以自带参数，

```python
import functools

def log(para):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('call %s() with %s' % (func.__name__, para))
            return func(*args, **kw)
        return wrapper
	return decorator
@log('text')
def foo():
    print('Hello')

@log('abc')
def bar():
    print('World')
    
foo()
bar()
```

还可以加入判断，实现带或者不带参数都可以的，

```python
import functools

def log(para):
    if isinstance(para, str):
        def decorator(func):
            @functools.wraps(func)
            def wrapper(*args, **kw):
                print('call %s() with %s' % (func.__name__, para))
                return func(*args, **kw)
            return wrapper
        return decorator
    else:
        # 从输出结果可以看到，这里的print先执行
        # 最后bar()时才输出'World'
        print(type(para))
        @functools.wraps(para)
        def wrapper(*args, **kw):
            return para(*args, **kw)
        return wrapper
    
@log('text')
def foo():
    print('Hello')

@log
def bar():
    print('World')
    
foo()
bar()
```

输出为

```python
<class 'function'>
call foo() with text
Hello
World
```

# 常用实例

计算函数运行时间的例子，

```python
import time, functools

def metric(fn):
    @functools.wraps(fn)
    def wrapper(*args, **kw):
        start = time.time()
        ret = fn(*args, **kw)
        end = time.time()
        print('%s executed in %s ms' % (fn.__name__, (end - start)))
        return ret
    return wrapper


# 测试
@metric
def fast(x, y):
    time.sleep(0.0012)
    return x + y;

@metric
def slow(x, y, z):
    time.sleep(0.1234)
    return x * y * z;

f = fast(11, 22)
s = slow(11, 22, 33)
```

输出结果，

```python
fast executed in 0.002000093460083008 ms
slow executed in 0.12399983406066895 ms
```

