---
title: C-01-data-type
date: 2019-01-06 14:17:04
categories: C
tags:
- STM32
- data type

---

# data type in STM32

在stdint.h 里放着C语言的标准表达方式

下面截取第36行开始的内容：

```C
typedef   signed  char         int8_t;
typedef   signed short int     int16_t;
typedef   signed  int          int32_t;
typedef   signed __int64       int64_t;
 
typedef   unsigned char         uint8_t;
typedef   unsigned short int    uint16_t;
typedef   unsigned int          uint32_t;
typedef   unsigned  __int64     uint64_t;
```

另外，在stm32f10x.h 有如下定义，可能是为了向下兼容

```C
typedef   uint32_t   u32;///32位
typedef   uint16_t   u16;///16位
typedef   uint8_t     u8;///8位
```

# Reference

https://blog.csdn.net/weibo1230123/article/details/80703788