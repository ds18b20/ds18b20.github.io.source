---
title: Python-15-FunctionParameter
date: 2018-02-16 12:59:47
categories: Python
tags:
- Python
- Function
- Parameter
---

Python-Function Parameters

位置参数，固定参数，可变参数，关键字参数

# 位置参数

在函数的括号里不加任何修饰地写入的参数就是位置参数。

```python
def power(x, n):
  s = 1
  while n > 0:
    n = n - 1
    s = s * x
  return s

power(2, 3)
```

# 默认参数

一部分参数经常变化，另一部分很少变化时，可以使用默认参数。

```python
def power(x, n=2):
  s = 1
  while n > 0:
    n = n - 1
    s = s * x
  return s

# 相当于调用power(2, 2)
power(2)
```

注意：默认参数必须指向不变对象，例如integer/float/str/None

# 可变参数

通过加入*符号实现可变参数，即可以同时传入0-n个参数。

```python
def calc(nums):
  s = 0
  for i in nums:
        s += i * i
  return s

# 需要先把左右参数组成一个list或tuple
a = [1,2,3,4,5,6]
calc(a)
```

可以这种把一个list或者tuple作为参数整体传递的情况可以使用可变参数来实现。传入的参数在函数内部组成一个tuple来使用。

```python
def calc(*nums):
  s = 0
  for i in nums:
        s += i * i
  return s

# 直接列出所有参数即可
# 在函数内部所有传入的参数组成了一个名为nums的tuple
calc(1，2，3，4，5，6)
```

如果已经有了一个list或者tuple，可以这样使用可变参数：

```python
def calc(*nums):
  s = 0
  for i in nums:
        s += i * i
  return s

# 实参前加上*将list/tuple转换成可变参数传入
a = [1,2,3,4,5,6]
calc(*a)
```

# 关键字参数

通过在参数前加入**符号来实现关键字参数。在函数内部把所有传入的“参数名=值”转换成一个dict来使用。

```python
def person(name, age, **kw):
  print('name:',name, 'age:',age, 'kw:',kw)

# 仅使用固定参数
person('Michael', 28)
# 使用固定参数+kw参数
# 注意此处city和tel是参数名，还不是dict的key，故不要加''
person('Michael', 28, city='Beijing', tel='0123')
```

## 命名关键字参数

上述关键字参数中，可以传入任意数目且内容不受限的关键字对。内部可以使用

```python
def person(name, age, **kw):
  if 'city' in kw:
    pass
  if 'tel' in kw:
    pass
```

来检查输入参数。

还可以使用“命名关键字参数”实现对参数的指定。使用方法为：

```python
def person(name, age, *, city, tel):
    print(name, age, city, tel)

# 这句的调用方式出错
# 命名关键字参数必须传入参数名
# 不然会当成位置参数来处理
person('John', 28, 'SH', '0123')
# 下面这样OK
person('John', 28, city='SH', tel='0123')
```

如上，命名关键字参数**必须传入参数名**。

命名关键字参数可以有缺省值（中的默认参数），从而简化调用。

```python
def person(name, age, *, city='Beijing', tel):
    print(name, age, city, tel)

# 可以不传入city参数
person('John', 28, tel='0123')
```

另外，如果参数中已经有了一个可变参数，后面的没有特殊标注的参数将被视为命名关键字参数。这时不需要再加入一个特殊分隔符*了：

```python
# args为可变参数
def person(name, age, *args, city, tel):
    print(name, age, args, city, tel)
    
person('John', 28, 12.5, '556', city='SH', tel='0123')
```

# 参数的组合

参数的组合顺序必须遵循：

位置参数---默认参数---可变参数---命名关键字参数---关键字参数

例如，

```python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```

