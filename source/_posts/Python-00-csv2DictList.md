---
title: Python-00-csv2DictList
date: 2018-03-10 12:25:31
categories: Python
tags:
- csv
- dict
- list
---



这篇主要是关于csv文件和dict/list类型数据的互相转换。

涉及到了以下常用的5种场景：

1. 将一个二重列表[[],[]]写入到csv文件中
2. 从csv文件中读取返回为列表
3. 将一字典写入到csv文件中
4. 从csv文件中读取一个字典
5. 从csv文件中读取一个计数字典(统计各key出现的次数)
6. 嵌套多个字典的列表写到csv中

# list 2 csv

```python
# 功能：将一个二重列表写入到csv文件中
# 输入：文件名称，数据列表
def createListCSV(fileName="", dataList=[]):
    with open(fileName, "wb") as csvFile:
        csvWriter = csv.writer(csvFile)
        for data in dataList:
            csvWriter.writerow(data)
```

# csv 2 list

```python
# 功能：从文本文件中读取返回为列表的形式
# 输入：文件名称，分隔符（默认,）
def readListCSV(fileName="", splitsymbol=","):
    dataList = []
    with open(fileName, "r") as csvFile:
        dataLine = csvFile.readline().strip("\n")
        while dataLine != "":
            tmpList = dataLine.split(splitsymbol)
            dataList.append(tmpList)
            dataLine = csvFile.readline().strip("\n")
    return dataList
```

# dict 2 csv

```python
# 功能：将一字典写入到csv文件中
# 输入：文件名称，数据字典
def createDictCSV(fileName="", dataDict={}):
    with open(fileName, "wb") as csvFile:
        csvWriter = csv.writer(csvFile)
        for k,v in dataDict.iteritems():
            csvWriter.writerow([k,v])
```

# csv 2 dict

```python
# 功能：从csv文件中读取一个字典
# 输入：文件名称，keyIndex,valueIndex
def readDictCSV(fileName="", keyIndex=0, valueIndex=1):
    dataDict = {}
    with open(fileName, "r") as csvFile:
        dataLine = csvFile.readline().strip("\n")
        while dataLine != "":
            tmpList = dataLine.split(splitsymbol)
            dataDict[tmpList[keyIndex]] = tmpList[valueIndex]
            dataLine = csvFile.readline().strip("\n")
    return dataDict
```

# csv get key num.

```python
# 功能：从csv文件中读取一个计数字典
# 输入：文件名称，keyIndex
def readDictCSV(fileName="", keyIndex=0):
    dataDict = {}
    with open(fileName, "r") as csvFile:
        dataLine = csvFile.readline().strip("\n")
        while dataLine != "":
            tmpList = dataLine.split(splitsymbol)
            if dataDict.get(tmpList[keyIndex]) == None:
                dataDict[tmpList[keyIndex]] = 0
            dataDict[tmpList[keyIndex]] = dataDict.get(tmpList[keyIndex]) + 1
            dataLine = csvFile.readline().strip("\n")
    return dataDict
```

# nested list 2 csv

嵌套多个字典的列表

```python
my_list = [{'players.vis_name': 'Khazri', 'players.role': 'Midfielder', 'players.country': 'Tunisia',
            'players.last_name': 'Khazri', 'players.player_id': '989', 'players.first_name': 'Wahbi',
            'players.date_of_birth': '08/02/1991', 'players.team': 'Bordeaux'},
           {'players.vis_name': 'Khazri', 'players.role': 'Midfielder', 'players.country': 'Tunisia',
            'players.last_name': 'Khazri', 'players.player_id': '989', 'players.first_name': 'Wahbi',
            'players.date_of_birth': '08/02/1991', 'players.team': 'Sunderland'},
           {'players.vis_name': 'Lewis Baker', 'players.role': 'Midfielder', 'players.country': 'England',
            'players.last_name': 'Baker', 'players.player_id': '9574', 'players.first_name': 'Lewis',
            'players.date_of_birth': '25/04/1995', 'players.team': 'Vitesse'}
           ]
```

转 csv的代码，

```python
# write nested list of dict to csv
def nestedlist2csv(list, out_file):
    with open(out_file, 'wb') as f:
        w = csv.writer(f)
        fieldnames=list[0].keys()  # solve the problem to automatically write the header
        w.writerow(fieldnames)
        for row in list:
            w.writerow(row.values())
```



# 输出csv中包含空白行的问题

问题在于这一句：open(fileName, "w")

在后面加入参数`newline=''`即可解决。

得到：open(fileName, "w", newline='')

# 参考

python列表、字典与csv

https://www.cnblogs.com/moye13/p/5291156.html

pythonでcsvファイルを整形したいが、一行間隔で空行を出力してしまいハマった話

http://helloworldryo.hatenablog.com/entry/2014/05/07/031628

csv文件与字典，列表等之间的转换小结【Python】

https://segmentfault.com/a/1190000007092479