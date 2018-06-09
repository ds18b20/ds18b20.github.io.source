---
title: Python-35-pywin32
date: 2018-05-03 23:10:06
categories: Python
tags:
- pywin32
---

# pywin32

pywin32是一个python下的库，它使得python能够访问Windows API的扩展，提供了齐全的Windows常量、接口、线程以及COM机制等等。

# 安装

从此处下载已经编译好的pywin32库：https://www.lfd.uci.edu/~gohlke/pythonlibs/#pywin32

然后cmd下cd切换到上述库的下载保存位置，然后执行`pip install FILE_NAME.whl`若没有错误提示，则安装成功。

##参考

https://stackoverflow.com/questions/20113456/installing-win32gui-python-module

# 使用

例1.通过类名和标题查找窗口句柄，并获得窗口位置和大小 

```python
import win32gui
import win32api

classname = "MozillaWindowClass"
titlename = "百度一下，你就知道 - Mozilla Firefox"
#获取句柄
hwnd = win32gui.FindWindow(classname, titlename)
#获取窗口左上角和右下角坐标
left, top, right, bottom = win32gui.GetWindowRect(hwnd)
```

2.通过父句柄获取子句柄 

3.鼠标定位与点击 

4.发送回车键 

5.关闭窗口 

## 参考

python和pywin32实现窗口查找、遍历和点击

https://www.cnblogs.com/zoro-robin/p/5591185.html