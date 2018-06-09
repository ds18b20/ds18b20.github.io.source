---
title: Numpy-19-float64
date: 2018-04-22 10:15:30
categories: Python
tags:
- Numpy
- float64
---

# Numpy的数据类型

Numpy中最大能表示的浮点数为float64，超过这个数值的结算结果会导致overflow溢出。

以下为Numpy官方对于Data types的定义：

NumPy supports a much greater variety of numerical types than Python does. This section shows which are available, and how to modify an array’s data-type.

| Data type  | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| `bool_`    | Boolean (True or False) stored as a byte                     |
| `int_`     | Default integer type (same as C `long`; normally either `int64` or `int32`) |
| intc       | Identical to C `int` (normally `int32` or `int64`)           |
| intp       | Integer used for indexing (same as C `ssize_t`; normally either `int32` or `int64`) |
| int8       | Byte (-128 to 127)                                           |
| int16      | Integer (-32768 to 32767)                                    |
| int32      | Integer (-2147483648 to 2147483647)                          |
| int64      | Integer (-9223372036854775808 to 9223372036854775807)        |
| uint8      | Unsigned integer (0 to 255)                                  |
| uint16     | Unsigned integer (0 to 65535)                                |
| uint32     | Unsigned integer (0 to 4294967295)                           |
| uint64     | Unsigned integer (0 to 18446744073709551615)                 |
| `float_`   | Shorthand for `float64`.                                     |
| float16    | Half precision float: sign bit, 5 bits exponent, 10 bits mantissa |
| float32    | Single precision float: sign bit, 8 bits exponent, 23 bits mantissa |
| float64    | Double precision float: sign bit, 11 bits exponent, 52 bits mantissa |
| `complex_` | Shorthand for `complex128`.                                  |
| complex64  | Complex number, represented by two 32-bit floats (real and imaginary components) |
| complex128 | Complex number, represented by two 64-bit floats (real and imaginary components) |

https://docs.scipy.org/doc/numpy-1.13.0/user/basics.types.html

# float64的表示范围

注意到float64的取值范围为Double precision float: sign bit, 11 bits exponent, 52 bits mantissa

exponent: 指数

mantissa: 尾数

故，float64能表示的最大范围约为+-2*2^2048

具体计算过程参考链接：

关于float，double等表示的数值范围的计算

https://blog.csdn.net/wlimin0523/article/details/34420515



实际测试过程中，np.exp(709)输出正常，np.exp(710)输出inf，已经溢出。

故使用numpy做e的指数运算时，不能超过709。