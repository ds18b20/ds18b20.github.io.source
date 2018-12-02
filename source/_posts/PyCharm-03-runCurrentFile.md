---
title: PyCharm-03-runCurrentFile
date: 2018-08-19 00:33:47
categories: PyCharm
tags:
- run
- current file

---

# PyCharm run current file

通常在PyCharm下运行Python代码需要进入右上角的`Edit Configuration`设置Python版本以及要运行的脚本文件，然后执行运行。
如果要临时测试运行另一个文件，反复去切换设置比较浪费时间，可以直接找到左侧导航栏内的当前文件，右键选择`Run 'your-file-name'`。
当然也可以直接使用该功能的快捷键`⌃（control）+⇧（shift）+R`。要修改该快捷键的配置直接打开PyCharm的Preference，进入keymap选项下查找到`run context configuration`，然后就可以修改其快捷键设置了。
如下图：
![](PyCharm-03-runCurrentFile\PyCharm_run.png)

# Reference

https://stackoverflow.com/questions/43807305/how-can-i-run-the-current-file-in-pycharm

