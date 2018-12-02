---
title: Python-54-findinlist
date: 2018-10-07 21:35:38
categories: Python
tags:
- find in list

---

# find in list

在Python中常会遇到，如何判断某一元素是否在list中；或者从一个list中找到某个元素的位置。

## 判断元素是否存在

最常用的方法是使用`in`语法，例如`2 in [1, 2, 3]`。

## 筛选出目标元素

可以使用列表生成的方式列出目标，如果不为空则包含目标元素：

```python
matches = [x for x in lst if fulfills_some_condition(x)]
matches = (x for x in lst if x > 6)
```

其中第一种方法在Python3中完全可以使用filter方法来实现：

```python
matches = filter(fulfills_some_condition, lst)
```

## 找出第一个符合条件的元素

可以loop所有元素直到找到目标元素，否则抛出异常。或者可以使用：

```python
next(x for x in lst if ...)
```

它会返回第一个目标元素，否则抛出错误`StopIteration`。亦或者，

```python
next((x for x in lst if ...), [default value])
```

## 找出目标元素的位置

使用list的index方法可以返回目标第一次出现时的位置。

```python
[1,2,3].index(2) # => 1
[1,2,3].index(4) # => ValueError
```

但是，不能返回后续出现的相同元素。这时可以使用enumerate方法：

```python
[idx for idx, x in enumerate([1,2,3,2]) if x==2] # => [1, 3]
```

# Reference

以下是原回答：

As for your first question: that code is perfectly fine and should work if `item` equals one of the elements inside `myList`. Maybe you try to find a string that does not *exactly* match one of the items or maybe you are using a float value which suffers from inaccuracy.

As for your second question: There's actually several possible ways if "finding" things in lists.

### Checking if something is inside

This is the use case you describe: Checking whether something is inside a list or not. As you know, you can use the `in` operator for that:

```py
3 in [1, 2, 3] # => True
```

### Filtering a collection

That is, finding all elements in a sequence that meet a certain condition. You can use list comprehension or generator expressions for that:

```py
matches = [x for x in lst if fulfills_some_condition(x)]
matches = (x for x in lst if x > 6)
```

The latter will return a *generator* which you can imagine as a sort of lazy list that will only be built as soon as you iterate through it. By the way, the first one is exactly equivalent to

```py
matches = filter(fulfills_some_condition, lst)
```

in Python 2. Here you can see higher-order functions at work. In Python 3, `filter` doesn't return a list, but a generator-like object.

### Finding the first occurrence

If you only want the first thing that matches a condition (but you don't know what it is yet), it's fine to use a for loop (possibly using the `else` clause as well, which is not really well-known). You can also use

```py
next(x for x in lst if ...)
```

which will return the first match or raise a `StopIteration` if none is found. Alternatively, you can use

```py
next((x for x in lst if ...), [default value])
```

### Finding the location of an item

For lists, there's also the `index` method that can sometimes be useful if you want to know *where* a certain element is in the list:

```py
[1,2,3].index(2) # => 1
[1,2,3].index(4) # => ValueError
```

However, note that if you have duplicates, `.index` always returns the lowest index:......

```py
[1,2,3,2].index(2) # => 1
```

If there are duplicates and you want all the indexes then you can use `enumerate()` instead:

```py
[i for i,x in enumerate([1,2,3,2]) if x==2] # => [1, 3]
```

[find in list](https://stackoverflow.com/questions/9542738/python-find-in-list)

