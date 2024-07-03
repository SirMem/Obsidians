---
Project: "[[Java]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-06-23
Connected:
---

在Java编程语言中，类型系统是非常重要的基础概念。Java的类型系统分为两大类：**基本类型**（也称为原始类型）和**引用类型**。下面我们将详细讲解这两大类类型及其相关概念。

  

### 1. 基本类型（Primitive Types）

  

基本类型是Java中最基本的数据类型，它们直接存储在内存中。Java有8种基本类型：

  

#### 整数类型

1. **byte**：8位有符号整数，取值范围是 -128 到 127。

   ```java

   byte b = 100;

   ```

2. **short**：16位有符号整数，取值范围是 -32,768 到 32,767。

   ```java

   short s = 10000;

   ```

3. **int**：32位有符号整数，取值范围是 -2^31 到 2^31-1。

   ```java

   int i = 100000;

   ```

4. **long**：64位有符号整数，取值范围是 -2^63 到 2^63-1。

   ```java

   long l = 10000000000L;

   ```

  

#### 浮点类型

5. **float**：32位单精度浮点数。

   ```java

   float f = 10.5f;

   ```

6. **double**：64位双精度浮点数。

   ```java

   double d = 10.5;

   ```

  

#### 字符类型

7. **char**：16位Unicode字符，取值范围是 '\u0000' (0) 到 '\uffff' (65,535)。

   ```java

   char c = 'A';

   ```

  

#### 布尔类型

8. **boolean**：表示真或假，取值范围是 `true` 或 `false`。

   ```java

   boolean flag = true;

   ```

  

### 2. 引用类型（Reference Types）

  

引用类型包括类、接口、数组和枚举。引用类型的变量存储的是对象的引用（地址），而不是对象本身。

  

#### 类（Class）

类是Java中最基本的引用类型。类定义了对象的属性和行为。

  

```java

public class Person {

    String name;

    int age;

  

    public Person(String name, int age) {

        this.name = name;

        this.age = age;

    }

  

    public void introduce() {

        System.out.println("Hello, my name is " + name + " and I am " + age + " years old.");

    }

}

  

public class Main {

    public static void main(String[] args) {

        Person person = new Person("Alice", 30);

        person.introduce();

    }

}

```

  

#### 接口（Interface）

接口是引用类型的一种，它定义了一组方法，但不提供实现。类可以实现接口，提供接口方法的具体实现。

  

```java

public interface Animal {

    void makeSound();

}

  

public class Dog implements Animal {

    public void makeSound() {

        System.out.println("Woof");

    }

}

  

public class Main {

    public static void main(String[] args) {

        Animal dog = new Dog();

        dog.makeSound();

    }

}

```

  

#### 数组（Array）

数组是存储相同类型元素的容器。数组的长度是固定的，一旦创建就不能改变。

  

```java

public class Main {

    public static void main(String[] args) {

        int[] intArray = {1, 2, 3, 4, 5};

        String[] strArray = {"apple", "banana", "cherry"};

  

        System.out.println(intArray[0]); // 输出第一个元素

        System.out.println(strArray[1]); // 输出第二个元素

    }

}

```

  

#### 枚举（Enum）

枚举是一种特殊的类，表示一组固定的常量。

  

```java

public enum Day {

    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY

}

  

public class Main {

    public static void main(String[] args) {

        Day today = Day.MONDAY;

        System.out.println("Today is " + today);

    }

}

```

  

### 3. 类型转换（Type Conversion）

  

#### 自动类型转换（Widening Conversion）

自动类型转换是从一个较小范围的类型转换为较大范围的类型，例如从`int`转换为`long`。

  

```java

int i = 100;

long l = i; // 自动类型转换

```

  

#### 强制类型转换（Narrowing Conversion）

强制类型转换是从一个较大范围的类型转换为较小范围的类型，需要显式进行。

  

```java

long l = 100L;

int i = (int) l; // 强制类型转换

```

  

#### 包装类（Wrapper Classes）

Java为每种基本类型提供了对应的包装类，这些包装类位于`java.lang`包中，可以将基本类型转换为对象类型。

  

```java

int i = 100;

Integer integer = Integer.valueOf(i); // 基本类型转换为包装类对象

int j = integer.intValue(); // 包装类对象转换为基本类型

```

  

### 4. 泛型类型（Generic Types）

  

泛型类型允许你在定义类、接口或方法时使用类型参数，使代码能够处理多种类型的数据。

  

```java

public class Box<T> {

    private T content;

  

    public void setContent(T content) {

        this.content = content;

    }

  

    public T getContent() {

        return content;

    }

}

  

public class Main {

    public static void main(String[] args) {

        Box<Integer> intBox = new Box<>();

        intBox.setContent(123);

        System.out.println(intBox.getContent());

  

        Box<String> strBox = new Box<>();

        strBox.setContent("Hello");

        System.out.println(strBox.getContent());

    }

}

```

  

### 5. 类型推导（Type Inference）

  

在Java 10及以后的版本中，引入了`var`关键字，可以由编译器自动推导变量的类型，简化代码编写。

  

```java

public class Main {

    public static void main(String[] args) {

        var i = 100; // 编译器推导出 i 的类型为 int

        var str = "Hello"; // 编译器推导出 str 的类型为 String

        System.out.println(i);

        System.out.println(str);

    }

}

```

  

### 6. 总结

  

Java的类型系统包括基本类型和引用类型。基本类型直接存储在内存中，有8种；引用类型包括类、接口、数组和枚举，存储的是对象的引用。Java提供了自动类型转换和强制类型转换，以及包装类和泛型类型来增强类型系统的灵活性。类型推导进一步简化了代码编写。

  

通过理解和掌握Java的类型系统，可以编写更加类型安全和高效的代码。


  

### 包装类的基本概念

包装类（Wrapper Classes）在Java中用于将基本类型（如`int`、`double`等）转换为对象类型，这在需要对象而不是基本类型的场景中非常有用。每个基本类型都有一个对应的包装类，这些包装类提供了许多有用的方法和常量。下面我们详细讲解包装类的使用方法及其常用方法。


Java中的每个基本类型都有一个对应的包装类：

  

| 基本类型 | 包装类       |

|----------|--------------|

| byte     | Byte         |

| short    | Short        |

| int      | Integer      |

| long     | Long         |

| float    | Float        |

| double   | Double       |

| char     | Character    |

| boolean  | Boolean      |

  

### 包装类的使用方法

  

#### 1. 基本类型与包装类之间的转换

  

##### 自动装箱（Autoboxing）

自动装箱是指将基本类型自动转换为对应的包装类对象。

  

```java

int i = 100;

Integer integer = i; // 自动装箱

```

  

##### 自动拆箱（Unboxing）

自动拆箱是指将包装类对象自动转换为对应的基本类型。

  

```java

Integer integer = 100;

int i = integer; // 自动拆箱

```

  

#### 2. 手动装箱与拆箱

  

##### 手动装箱

手动装箱是通过包装类的构造函数或静态方法将基本类型转换为包装类对象。

  

```java

int i = 100;

Integer integer1 = new Integer(i); // 使用构造函数（不推荐，已过时）

Integer integer2 = Integer.valueOf(i); // 使用静态方法

```

  

##### 手动拆箱

手动拆箱是通过包装类的实例方法将包装类对象转换为基本类型。

  

```java

Integer integer = 100;

int i = integer.intValue(); // 使用实例方法

```

  

### 包装类的常用方法

  

包装类提供了许多有用的方法，用于基本类型和字符串之间的转换、比较等。下面以`Integer`类为例，详细介绍一些常用方法。

  

#### `Integer`类的常用方法

  

1. **`valueOf(int i)`**：将基本类型`int`转换为`Integer`对象。

   ```java

   Integer integer = Integer.valueOf(100);

   ```

  

2. **`valueOf(String s)`**：将字符串转换为`Integer`对象。

   ```java

   Integer integer = Integer.valueOf("100");

   ```

  

3. **`parseInt(String s)`**：将字符串转换为基本类型`int`。

   ```java

   int i = Integer.parseInt("100");

   ```

  

4. **`toString()`**：将`Integer`对象转换为字符串。

   ```java

   Integer integer = 100;

   String str = integer.toString();

   ```

  

5. **`intValue()`**：将`Integer`对象转换为基本类型`int`。

   ```java

   Integer integer = 100;

   int i = integer.intValue();

   ```

  

6. **`compareTo(Integer anotherInteger)`**：比较两个`Integer`对象。

   ```java

   Integer integer1 = 100;

   Integer integer2 = 200;

   int comparison = integer1.compareTo(integer2); // 返回负数，零，或正数

   ```

  

7. **`equals(Object obj)`**：判断两个`Integer`对象是否相等。

   ```java

   Integer integer1 = 100;

   Integer integer2 = 100;

   boolean isEqual = integer1.equals(integer2); // 返回 true

   ```

  

8. **`MAX_VALUE`**：表示`int`类型的最大值。

   ```java

   int max = Integer.MAX_VALUE;

   ```

  

9. **`MIN_VALUE`**：表示`int`类型的最小值。

   ```java

   int min = Integer.MIN_VALUE;

   ```

  

### 包装类的详细示例

  

以下是一个详细的示例，演示了包装类的各种使用方法和属性：

  

```java

public class WrapperClassExample {

    public static void main(String[] args) {

        // 自动装箱

        Integer autoBoxedInt = 100;

        System.out.println("Auto-boxed Integer: " + autoBoxedInt);

  

        // 自动拆箱

        int autoUnboxedInt = autoBoxedInt;

        System.out.println("Auto-unboxed int: " + autoUnboxedInt);

  

        // 手动装箱

        Integer manuallyBoxedInt1 = new Integer(200); // 使用构造函数（不推荐，已过时）

        Integer manuallyBoxedInt2 = Integer.valueOf(200); // 使用静态方法

        System.out.println("Manually boxed Integer (constructor): " + manuallyBoxedInt1);

        System.out.println("Manually boxed Integer (valueOf): " + manuallyBoxedInt2);

  

        // 手动拆箱

        int manuallyUnboxedInt = manuallyBoxedInt2.intValue();

        System.out.println("Manually unboxed int: " + manuallyUnboxedInt);

  

        // 常量属性

        System.out.println("Integer Max Value: " + Integer.MAX_VALUE);

        System.out.println("Integer Min Value: " + Integer.MIN_VALUE);

        System.out.println("Double Max Value: " + Double.MAX_VALUE);

        System.out.println("Double Min Value: " + Double.MIN_VALUE);

  

        // 静态方法

        int parsedInt = Integer.parseInt("123");

        String intAsString = Integer.toString(123);

        System.out.println("Parsed int: " + parsedInt);

        System.out.println("Integer as String: " + intAsString);

  

        // 比较两个包装类对象

        Integer integer1 = 100;

        Integer integer2 = 200;

        int comparison = Integer.compare(integer1, integer2); // 返回负数，零，或正数

        System.out.println("Comparison result: " + comparison);

  

        // 实例方法

        boolean isEqual = integer1.equals(integer2); // 返回 false

        System.out.println("Are integers equal? " + isEqual);

    }

}