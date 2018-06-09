---
title: Python-17-str-format
date: 2018-02-16 16:45:33
categories: Python
tags:
- Python
- String
- Format
---

# 格式化操作符 %

针对各种类型的数据，有不同的操作符

| 格式化符号 | 说明                                                         |
| :--------- | ------------------------------------------------------------ |
| %c         | 转换成字符（ASCII 码值，或者长度为一的字符串）               |
| %r         | 优先用repr()函数进行字符串转换                               |
| %s         | 优先用str()函数进行字符串转换                                |
| %d / %i    | 转成有符号十进制数                                           |
| %u         | 转成无符号十进制数                                           |
| %o         | 转成无符号八进制数                                           |
| %x / %X    | 转成无符号十六进制数（x / X 代表转换后的十六进制字符的大小写） |
| %e / %E    | 转成科学计数法（e / E控制输出e / E）                         |
| %f / %F    | 转成浮点数（小数部分自然截断）                               |
| %g / %G    | %e和%f / %E和%F 的简写                                       |
| %%         | 输出% （格式化字符串里面包括百分号，那么必须使用%%）         |

其中 %s 和 %r 的区别需要特别强调一下，

```python
string = "Hello\tWill\n"

print("%s" % string)
print("%r" % string)
```

输出结果为

```python
Hello	Will

'Hello\tWill\n'
```

区别一目了然，其实差异来自于str()和repr()两个函数之间的差别：

- str()得到的字符串是面向用户的，具有较好的可读性
- repr()得到的字符串是面向机器的
  - 通常（不是所有）repr()得到的效果是：obj == eval(repr(obj))

注意，同时输出多个对象时，需先组成一个tuple来使用。

# 格式化操作符辅助符

结合上述格式化操作符一起使用：

| **辅助符号** | **说明**                                                     |
| ------------ | ------------------------------------------------------------ |
| *****        | 定义宽度或者小数点精度                                       |
| **-**        | 用做左对齐                                                   |
| **+**        | 在正数前面显示加号(+)                                        |
| **#**        | 在八进制数前面显示零(0)，在十六进制前面显示"0x"或者"0X"（取决于用的是"x"还是"X"） |
| **0**        | 显示的数字前面填充"0"而不是默认的空格                        |
| **(var)**    | 映射变量（通常用来处理字段类型的参数）                       |
| **m.n**      | m 是显示的最小总宽度，n 是小数点后的位数（如果可用的话）     |

例子

```python
num = 100

print "%d to hex is %x" %(num, num)
print "%d to hex is %X" %(num, num)
print "%d to hex is %#x" %(num, num)
print "%d to hex is %#X" %(num, num) 

# 浮点数
f = 3.1415926
print "value of f is: %.4f" %f

# 指定宽度和对齐
students = [{"name":"Wilber", "age":27}, {"name":"Will", "age":28}, {"name":"June", "age":27}]
print "name: %10s, age: %10d" %(students[0]["name"], students[0]["age"])
print "name: %-10s, age: %-10d" %(students[1]["name"], students[1]["age"])
print "name: %*s, age: %0*d" %(10, students[2]["name"], 10, students[2]["age"])

# dict参数
for student in students:
print "%(name)s is %(age)d years old" %student
```

对于Python的格式化操作符，不仅可以接受tuple类型的参数，也可以支持dict，象上面代码的最后一部分，那么格式化字符串中就可以直接使用"%(key)s"（这里的s根据具体类型改变）的方式表示dict中对应的value了。

# 字符串模板

目前还未使用过，用前需要先引入string模块

```python
from string import Template

s = Template("Hi, $name! $name is learning $language")
print s.substitute(name="Wilber", language="Python")

d = {"name": "Will", "language": "C#"}
print s.substitute(d)

# 用$$表示$符号
s = Template("This book ($bname) is 17$$")
print s.substitute(bname="TCP/IP")
```

# 字符串内建函数format()

## 基本使用

从Python 2.6开始新增字符串格式化函数format，使用{}当作操作符。

```python
# 位置参数
print "{0} is {1} years old".format("Wilber", 28)
print "{} is {} years old".format("Wilber", 28)
print "Hi, {0}! {0} is {1} years old".format("Wilber", 28)

# 关键字参数
print "{name} is {age} years old".format(name = "Wilber", age = 28)

# 下标参数
li = ["Wilber", 28]
print "{0[0]} is {0[1]} years old".format(li)

# 填充与对齐
# ^、<、>分别是居中、左对齐、右对齐，后面带宽度
# :号后面带填充的字符，只能是一个字符，不指定的话默认是用空格填充
print '{:>8}'.format('3.14')
print '{:<8}'.format('3.14')
print '{:^8}'.format('3.14')
print '{:0>8}'.format('3.14')
print '{:a>8}'.format('3.14')

# 浮点数精度
print '{:.4f}'.format(3.1415926)
print '{:0>10.4f}'.format(3.1415926)

# 进制
# b、d、o、x分别是二进制、十进制、八进制、十六进制
print '{:b}'.format(11)
print '{:d}'.format(11)
print '{:o}'.format(11)
print '{:x}'.format(11)
print '{:#x}'.format(11)
print '{:#X}'.format(11)

# 千位分隔符
print '{:,}'.format(15700000000)
```

## use Tuple or Dict

可以使用tuple或dict来做format的参数，

```python
# Format with tuple
tmp_tuple = ('val0', 23.5)
print("Format string with {0[0]}, {0[1]}".format(tmp_tuple))
print("Format string with {}, {}".format(*tmp_tuple))
print("Format string with {1}, {0}".format(*tmp_tuple))
print("Format string with {0}, {1}, {0}".format(*tmp_tuple))

# Format with dict
# 注意此处key不能是纯数字的字符串，参见参考链接3
# 因为dict并不要求key必须是str，参见链接4
tmp_dict_0 = {'val0': 23.5, 'val1': 'xxx'}
tmp_dict_1 = {'val0': 12.6, 'val1': 'yyy'}
print("Format string with {0[val0]}, {1[val0]}".format(tmp_dict_0, tmp_dict_1))
print("Format string with {0[val0]:0.3f}".format(tmp_dict_0))
print("Format string with {val0:0.3f}".format(**tmp_dict_0))
```

输出，

```
Format string with val0, 23.5
Format string with val0, 23.5
Format string with 23.5, val0
Format string with val0, 23.5, val0
Format string with 23.5, 12.6
Format string with 23.500
Format string with 23.500
```

## 使用类的属性

```python
class Person:
    def __init__(self, name, age, gender):
        self.name = name
        self.age = age
        self.gender = gender
        
    def __str__(self):
        return '{self.name} is a {self.age} years old {self.gender}'.format(self=self)

str(Person('Tom', 26, 'man'))
```

输出，`'Tom is a 26 years old man'`

# str的内建函数

通过 dir(str) 来查看。

其中就有在其他文章中写的str.strip()函数。

# 参考

[Python格式化字符串](https://www.cnblogs.com/wilber2013/p/4641616.html)

[Python强大的格式化format](https://www.cnblogs.com/wongbingming/p/6848701.html)

[String formatting [str.format()] with a dictionary key which is a str() of a number](https://stackoverflow.com/questions/11130790/string-formatting-str-format-with-a-dictionary-key-which-is-a-str-of-a-num)

[5.5. Dictionaries：](https://docs.python.org/3/tutorial/datastructures.html#dictionaries)

> Unlike sequences, which are indexed by a range of numbers, dictionaries are indexed by *keys*, which can be any immutable type; strings and numbers can always be keys. Tuples can be used as keys if they contain only strings, numbers, or tuples; if a tuple contains any mutable object either directly or indirectly, it cannot be used as a key. You can’t use lists as keys, since lists can be modified in place using index assignments, slice assignments, or methods like `append()` and `extend()`. 

