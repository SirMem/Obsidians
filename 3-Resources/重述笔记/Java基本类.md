---
Project: "[[接口和实现类的关系]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-05-30
Connected: 
---

# `System`类的关键特性和成员

1. **静态字段**：
    - `System.out`：标准输出流，通常连接到控制台。
    - `System.err`：标准错误流，通常也连接到控制台，但通常用于输出错误信息。
    - `System.in`：标准输入流，通常连接到键盘输入。

2. **常用静态方法**：
    - `System.currentTimeMillis()`：返回当前时间的毫秒数，从1970年1月1日00:00:00 UTC开始计算。
    - `System.nanoTime()`：返回高精度时间源的当前值，适用于测量时间间隔。
    - `System.exit(int status)`：终止当前运行的Java虚拟机，参数为状态码。
    - `System.gc()`：请求JVM进行垃圾回收。
    - `System.getenv(String name)`：获取指定环境变量的值。
    - `System.getProperty(String key)`：获取系统属性的值。
    - `System.setProperty(String key, String value)`：设置系统属性的值。

## 示例代码
以下是一个使用`System`类的示例代码：

```java
package com.example;

public class SystemExample {
    public static void main(String[] args) {
        // 打印当前时间的毫秒数
        long currentTimeMillis = System.currentTimeMillis();
        System.out.println("Current Time Millis: " + currentTimeMillis);

        // 打印系统属性
        String javaVersion = System.getProperty("java.version");
        System.out.println("Java Version: " + javaVersion);

        // 打印环境变量
        String userHome = System.getenv("HOME");
        System.out.println("User Home: " + userHome);

        // 标准输出
        System.out.println("This is a standard output message.");

        // 错误输出
        System.err.println("This is an error message.");

        // 退出程序
        System.exit(0);
    }
}
```

## 关键点总结
- **`System`类**：包含多个静态字段和方法，用于与系统相关的操作。
- **标准输入/输出/错误**：`System.out`用于标准输出，`System.err`用于错误输出，`System.in`用于标准输入。
- **系统时间**：`System.currentTimeMillis()`和`System.nanoTime()`用于获取当前时间。
- **系统属性和环境变量**：`System.getProperty()`和`System.getenv()`用于访问系统属性和环境变量。

通过这些功能，`System`类为Java程序提供了与底层系统交互的多种方式。

**相关建议：**
**a.** 了解如何使用`System`类的其他方法，例如`System.arraycopy()`，并编写示例代码。
**b.** 研究JVM的垃圾回收机制，了解`System.gc()`方法的实际作用和使用场景。