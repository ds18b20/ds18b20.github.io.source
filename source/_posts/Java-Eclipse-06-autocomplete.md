---
title: Java-Eclipse-06-autocomplete
date: 2018-03-17 15:45:10
categories: Java
tags:
- Java
- eclipse
- Auto Complete
---

# 自动补全

默认情况下，Eclipse仅支持逗号后的自动补全，即输入逗号之后，根据逗号前面的内容提供用于补全的候选内容。

其实可以修改一下设置实现任何字母后的自动补全提示。

设置方法：

Eclipse窗口 > Windows > Preferences > Java > Editor > Content Assist

右侧下方找到Auto Activation，在Auto activation triggers for Java:设置处可以看到默认只有一个逗号。这就是只有逗号能触发自动补全的原因。

在逗号后面紧接着继续输入想要触发的字母就可以了。一般可以把大小写字母全部补上，即abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ

这样虽然方便，但是个人以为大写字母后的补全不常见，所以只加入了所有的小写字母。

另外，Auto activation delay也最好根据电脑性能做调整，数值太小会占用太多系统资源。这里，我设置的是300 ms延迟。

