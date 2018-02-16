---
title: Python-00-ListHalfOpen
date: 2018-02-13 22:27:17
categories: Python
tags:
- Python
- List
- Half Open
---

# Python 列表的两点思考

Python定义列表(list)时，可以直接用中括号把列表包含的所有元素添加进来，并用逗号隔开。

当内部元素较多，而且有某种规律时，另一种通过range()函数生成的方式更为直接。

在使用range(START,END,STEP)时得到的数组包含START，但是不包含END,即左闭右开（前闭后开）。

当引用列表内元素的时候，第一个元素用下标0表示。多数程序设计语言都是从0开始定位。

左闭右开 `range(start, end)`

```python
>>> list(range(2, 8))
[2, 3, 4, 5, 6, 7]
```

上述结果可知，包含2不包含8。这其中的原因，自己思考外加网上的观点如下，

1. 对于[START, END]，COUNT<>END - START，而是COUNT = END - START + 1，这就给书写和理解带来了困扰。同理(START, END)也不直观。
   而左开右闭和左闭右开则没有这个问题，COUNT = END - START 更符合正常人类的思考方式
2. 针对上述的一点补充，划分序列时
   [1,2),[3,4),[5,6)显然比[1,2],(3,4),[5,6]美观。
3. 另一点右侧不可为闭的原因是，计算机硬件中迭代器对于比较谁大谁小的运算很麻烦，而判断两者是否相等却很容易做到。
   这时，如果需要遍历一个闭区间[0, n]，那么需要迭代器实现比较大小的操作。即：i = 0; while(i <= n) do {process(i); i++;} 。而很多抽象的迭代器无法比较大小。
   如果要遍历一个左开右闭区间(0, n]，则只需要迭代器支持比较相等的操作即可。即：i = 0; while(i != n) do {process(i); i++;}

# 第一个元素下标从0开始

C语言中要获得数组a中元素的地址时候，如果数组下标从0开始，那么&a[b]可以直接写成&a+b*sizeof(*a)

不然，如果下标从1开始，那么&a[b]必须写成&a+(b-1)*sizeof(*a)

另外C语言中用for语句遍历数组时也是左开右闭原则。

for(i=1;i<n;i++){}

# 参考

python中的细节—左闭右开原则 - CSDN

http://blog.csdn.net/HDOJ_lin/article/details/78831868

为什么 Python 里面的 range 不包含上界？ - vczh的回答 - 知乎

https://www.zhihu.com/question/24883243/answer/29315532

评论区 - cppblog

http://www.cppblog.com/woaidongmao/archive/2008/12/25/70343.aspx

python中的细节—左闭右开原则

http://blog.csdn.net/HDOJ_lin/article/details/78831868

