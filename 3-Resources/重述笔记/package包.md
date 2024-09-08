---
Project: "[[接口和实现类的关系]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-05-30
Connected: 
---
在Java中，`package`关键字用于将一组相关的类、接口和子包进行组织和管理。通过使用包，可以避免命名冲突，并且可以控制类和接口的访问权限。包结构有助于组织代码，使其更加模块化和易于维护。

### 主要用途

1. **组织类和接口**：将相关的类和接口放在同一个包中，有助于逻辑上的组织和管理。
2. **命名空间管理**：避免类名冲突。即使不同包中有同名的类，也不会冲突。
3. **访问控制**：通过包，可以更好地控制类和成员的访问级别。例如，包访问权限（默认访问修饰符）允许包内的类相互访问，而对包外的类隐藏。

### 使用方式

在Java源文件的开头使用`package`关键字来声明包。例如：

```java
package com.example;

public class MyClass {
    public static void main(String[] args) {
        System.out.println("Hello from MyClass in com.example package!");
    }
}
```

这个声明表示`MyClass`类属于`com.example`包。

### 包的命名惯例

- **层次结构**：包名通常采用层次结构，用点号（`.`）分隔。通常使用公司的域名反转来作为包的前缀，例如`com.example`。
- **小写字母**：包名通常使用小写字母，以避免与类名混淆。

### 访问包中的类

要使用其他包中的类，需要使用`import`语句。例如：

```java
package com.example;

import java.util.Date;

public class ImportExample {
    public static void main(String[] args) {
        Date date = new Date();
        System.out.println("Current date: " + date);
    }
}
```

在上面的例子中，`import java.util.Date`语句导入了`Date`类，因此可以在代码中直接使用。

### 包示例结构

假设我们有以下包结构：
```
src/
  └── com/
      └── example/
          ├── MainEntry.java
          ├── MyClass.java
          └──
