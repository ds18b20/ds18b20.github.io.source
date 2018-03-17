---
title: Java-08-FileNotFoundException
date: 2018-03-17 22:36:55
categories: Java
tags:
- Java
- FileNotFoundException
---

# 文件操作的异常处理

代码中的相关语句，

```java
PrintStream ps = new PrintStream("./log.txt");
```

编译不通过，提示错误为，

> Exception in thread "main" java.lang.Error: Unresolved compilation problem: 
> 	Unhandled exception type FileNotFoundException
>
> 	at ***

字面理解就是打开文件可能会发生异常，必须对这些异常做出处理。

有两种解决方法：

一是，在函数后throws Exception信息。

```java
public static void main(String[] args) {
    try { 
        Scanner s = new Scanner(new FileReader("/Users/daniel/pr/java/readFile/myfile.txt")); 
    } catch (FileNotFoundException e) {
        //do something with e, or handle this case
    }
}
```

另一个是，try/catch语法结构。

```java
public static void main(String[] args) throws FileNotFoundException{
    Scanner s = new Scanner(new FileReader("/Users/daniel/pr/java/readFile/myfile.txt")); 
}
```

## Eclipse纠错提示

按Ctrl+1键会有纠错提示，弹出的提示其实就是上述两中方法。

# 参考

FileNotFoundException when creating a Scanner in Eclipse with Java

https://stackoverflow.com/questions/10452948/filenotfoundexception-when-creating-a-scanner-in-eclipse-with-java