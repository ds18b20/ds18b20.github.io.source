---
title: Java-07-Package-JUnitTest
date: 2018-03-17 12:42:56
categories: Java
tags:
- Java
- Package
- JUnit Testd

---

# 导入Unit Test单元测试

# JUnit Test单元测试

Eclipse的Package Explore中右键已编辑好的class名，单击 **New > JUnit Test Case**。New JUnit Test Case 向导的第一页将会打开。

单击 **Next** 接受默认值，出现 Test Methods 对话框，选择要生成测试的一个或多个方法。单击Finish完成，在Package Explore里可以看到生成的JUnit测试的java文件。

双击打开JUnit测试代码，找到要测试的方法并太你家测试内容。

下面是一个例子，

```java
@Test
public void testPerson() {
  Person p = new Person("Joe Q Author", 42, 173, 82, "Brown", "MALE");
  Logger l = Logger.getLogger(Person.class.getName());
  l.info("Name: " + p.getName());
  l.info("Age:"+ p.getAge());
  l.info("Height (cm):"+ p.getHeight());
  l.info("Weight (kg):"+ p.getWeight());
  l.info("Eye Color:"+ p.getEyeColor());
  l.info("Gender:"+ p.getGender());
  assertEquals("Joe Q Author", p.getName());
  assertEquals(42, p.getAge());
  assertEquals(173, p.getHeight());
  assertEquals(82, p.getWeight());
  assertEquals("Brown", p.getEyeColor());
  assertEquals("MALE", p.getGender());
}
```

# 运行测试

右键Package Explore中的JUnit测试代码文件，选择 **Run As** > **JUnit Test**。