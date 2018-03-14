---
title: Java-05-StringChar
date: 2018-03-14 21:14:24
categories: Java
tags:
- Java
- String
- Char
---



数据类型之字符串和字符

# String

- 字符串用双引号括起来表示
- 字符串内要显示"等特殊符号时，需要用反斜杠实现转义(escape)

注意，使用`System.out.print()`函数输出字符串时，可以把字符串和数字相加，被加到数字也会变成字符串。

```java
System.out.println("sum of " + 1 + 1 + 1);
```

输出，`sum of 111`

字符串/String/文字列

# Char

- 字符用单引号括起来表示
- 只能包含一个字符，转义符(反斜杠)也可以包含进去

字符串的本质就是integer，具体数值可以查询ASII表。所以

```java
System.out.println('a' + 1 + 1 + 1);
```

输出，`100`

如果不想数字变成字符串，而是以计算结果表示，则应给数字部分加上括号，

```java
System.out.println("sum of " + (1 + 1 + 1));
```

输出，`sum of 3`

字符/Char/文字

# 输出

无论是String还是Char都可以使用标准输出函数System.out.print()或者System.out.println()来输出。二者不同的是，带ln的输出时末尾自动换行。

