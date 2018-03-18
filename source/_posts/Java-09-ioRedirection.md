---
title: Java-09-ioRedirection
date: 2018-03-17 23:12:06
categories: Java
tags:
- Java
- io Redirection

---

# io重定向



一个简单的例子，

```java
PrintStream ps = new PrintStream("./log.txt");
System.setOut(ps); // 设置使用新的输出流
System.out.println("我会出现在项目根目录下的log.txt文件中~");
```

ing...

# back to standard io

You can get hold of the file descriptor for standard out through [`FileDescriptor.out`](http://docs.oracle.com/javase/8/docs/api/java/io/FileDescriptor.html#out). To reset standard out to print to console, you do

```
System.setOut(new PrintStream(new FileOutputStream(FileDescriptor.out)));
```

Another way is to simply hold on to the original object, as follows:

```java
PrintStream stdout = System.out;
System.setOut(new PrintStream(logFile));

// ...

System.setOut(stdout);                   // reset to standard output
```

# 参考

Java基础-重定向输出流

http://blog.csdn.net/Young4Dream/article/details/68948947



Resetting Standard output Stream

https://stackoverflow.com/questions/5339499/resetting-standard-output-stream