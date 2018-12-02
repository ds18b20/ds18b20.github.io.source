---
title: Pandas-01-SeriesDataFrame
date: 2018-10-07 21:53:50
categories: Pandas
tags:
- Series
- DataFrame

---

# Pandas基本数据结构

Series和DataFrame是Pandas的两种基本数据结构，下面分别介绍。

## Pandas.Series类型

Series类似于一维数组对象，包含一个数组的数据对象(values)，和一个与该数组相关联的数据标签，该标签被称为索引(index)。

可以使用pandas.Series.values和pandas.Series.index来分别获取数组数据和索引，使用pandas.Series.dtypes来获取数据类型。
参考: https://pda.readthedocs.io/en/latest/chp5.html

```python
# 导入 numpy和pandas
import numpy as np
import pandas as pd
from pandas import Series, DataFrame
```

```python
# pd.Series
obj = Series([4, 7, -5, 3])
obj
```

输出，

```
0    4
1    7
2   -5
3    3
dtype: int64
```

查看values属性，

```python
obj.values
```

输出，

```
array([ 4,  7, -5,  3], dtype=int64)
```

查看index属性，

```python
obj.index
```

输出，

```
RangeIndex(start=0, stop=4, step=1)
```

查看数据类型，

```python
obj.dtypes
```

输出，

```
dtype('int64')
```

关于Series数据的类型，当所有数据的类型统一时输出就是当前数据的类型；当数据类型不统一时Pandas自动选择能够代表所有类型的数据类型，比如同时包含integer和float时会统一使用float来统一化。

值得注意的是，当数据中包含string或者nan时，obj.dtypes统一格式为object。

```python
series = pd.Series([np.nan, 2, 3])
series.dtypes
```

输出，

```
dtype('float64')
```

添加index，

```python
obj2 = Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
obj2
```

输出，

```
d    4
b    7
a   -5
c    3
dtype: int64
```

### Slice Series

下面测试Pandas的Slice功能。

```python
# 使用数组slice
s = obj2[[0, 1]]
print(s)
```

输出，

```
d    4
b    7
dtype: int64
```

上文讲到Series可以看作一维数组，所以可以使用位置来引用。并且Series还可以使用索引值来引用，因此也可以把Series看成字典来处理。可以使用类似key的index来引用，而且可以使用in语法来判断index是否存在。

```python
# 使用索引slice
ss = obj2[['d', 'b']]
print(ss)

print('d' in obj2)
```

输出，

```
d    4
b    7
dtype: int64
True
```

使用布尔值索引，

```python
# 使用布尔数据集slice
sss = obj2[obj2>3]
print(sss)
```

输出，

```
d    4
b    7
dtype: int64
```

dict内部数据没有顺序，但是使用dict生成的Series则有顺序。

```python
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
obj3 = Series(sdata)
obj3
```

输出，

```
Ohio      35000
Oregon    16000
Texas     71000
Utah       5000
dtype: int64
```

### 添加index

```python
states = ['California', 'Ohio', 'Oregon', 'Texas']
obj4 = Series(sdata, index=states)
obj4
```

输出，注意在data中没有对应的California列：

```
California        NaN
Ohio          35000.0
Oregon        16000.0
Texas         71000.0
dtype: float64
```

在这种情况下， sdata 中的3个值被放在了合适的位置，但因为没有发现对应于 ‘California’ 的值，就出现了 NaN （不是一个数），这在pandas中被用来标记数据缺失或 NA 值。我使用“missing”或“NA”来表示数度丢失。在pandas中用函数 isnull 和 notnull 来检测数据丢失：

```python
pd.isnull(obj4)
```

输出，

```
California     True
Ohio          False
Oregon        False
Texas         False
dtype: bool
```

测试notnull，

```python
pd.notnull(obj4)
```

输出，

```
California    False
Ohio           True
Oregon         True
Texas          True
dtype: bool
```

Series对象内也包含该方法。

```python
obj4.isnull()
```

输出，

```
California     True
Ohio          False
Oregon        False
Texas         False
dtype: bool
```

## DataFrame类型

DataFrame可以看作是一个二维数组结构，它由多个共享同一个行索引的多个数据列组成。各个列有自己的名称和数据类型。下面使用字典来生成一个DataFrame：
(注意各dict内的各个数据列必须有相同的长度)

```python
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9]}
frame = DataFrame(data)
frame
```

输出，

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pop</th>
      <th>state</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.5</td>
      <td>Ohio</td>
      <td>2000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.7</td>
      <td>Ohio</td>
      <td>2001</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.6</td>
      <td>Ohio</td>
      <td>2002</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.4</td>
      <td>Nevada</td>
      <td>2001</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2.9</td>
      <td>Nevada</td>
      <td>2002</td>
    </tr>
  </tbody>
</table>

### 添加columns

生成DataFrame时，默认包含一定的顺序。另外，可以通过加入columns参数来指定列的排列顺序。

```python
frame1 = DataFrame(data, columns=['state', 'year', 'pop'])
frame1
```

输出，

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>state</th>
      <th>year</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Ohio</td>
      <td>2000</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ohio</td>
      <td>2001</td>
      <td>1.7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ohio</td>
      <td>2002</td>
      <td>3.6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nevada</td>
      <td>2001</td>
      <td>2.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nevada</td>
      <td>2002</td>
      <td>2.9</td>
    </tr>
  </tbody>
</table>

通过columns属性来查看columns列表。

```python
frame1.columns
```

输出，

```
Index(['state', 'year', 'pop'], dtype='object')

```

### 添加index

类似和Series一样，如果你传递了一个行，但在data中没有包含对应名称的列，结果中它会表示为NA值：

```python
# index 参数用于指定行索引
frame2 = DataFrame(data, columns=['state', 'year', 'pop', 'area'], index=['one', 'two', 'three', 'four', 'five'])
frame2
```

输出，

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>state</th>
      <th>year</th>
      <th>pop</th>
      <th>area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>Ohio</td>
      <td>2000</td>
      <td>1.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>two</th>
      <td>Ohio</td>
      <td>2001</td>
      <td>1.7</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>three</th>
      <td>Ohio</td>
      <td>2002</td>
      <td>3.6</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>four</th>
      <td>Nevada</td>
      <td>2001</td>
      <td>2.4</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>five</th>
      <td>Nevada</td>
      <td>2002</td>
      <td>2.9</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

###Slice DataFrame

和Series一样，在DataFrame中的一列可以通过字典记法或属性来检索：
(此时返回的列就是上文所述的一个Series，可以通过type查看)

```python
frame2['state']
```

输出，

```
0      Ohio
1      Ohio
2      Ohio
3    Nevada
4    Nevada
Name: state, dtype: object
```

使用属性方式，

```python
frame2.state
```

输出，

```
0      Ohio
1      Ohio
2      Ohio
3    Nevada
4    Nevada
Name: state, dtype: object
```

使用字典方式，

```python
type(frame2['state'])
```

输出，

```
pandas.core.series.Series
```

下面是行的索引方法，即取出满足条件的某一行的记录。

```python
frame2.ix['three']
```

```
C:\Users\ds18b20\Anaconda3\lib\site-packages\ipykernel\__main__.py:1: DeprecationWarning: 
.ix is deprecated. Please use
.loc for label based indexing or
.iloc for positional indexing

See the documentation here:
http://pandas.pydata.org/pandas-docs/stable/indexing.html#ix-indexer-is-deprecated
  if __name__ == '__main__':
```

输出，

```
state    Ohio
year     2002
pop       3.6
area      NaN
Name: three, dtype: object
```

最新的方式为使用loc和iloc，

```python
frame2.loc['three']
```

输出，

```
state    Ohio
year     2002
pop       3.6
area      NaN
Name: three, dtype: object
```

使用iloc，

```python
frame2.iloc[2]
```

输出，

```
state    Ohio
year     2002
pop       3.6
area      NaN
Name: three, dtype: object
```

