---
Project: "[[Mybatis入门]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 
Connected:
---

## 使用Mybatis 查询所有用户数据
1,准备工作(创建springboot工程、数据库表user、实体类User)
2.引入Mybatis的相关依赖，配置Mybatis(数据库连接信息)

```sql
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
spring.datasource.username=root
spring.datasource.password=1234

//application.properties
```
3.编写5QL语句(注解/XML)
持久层接口
```java
@Mapper
public interface UserMapper
@Select("select * from user")
public List<User>list();
```

使用Mapper注解后不需要定义接口的实现类，程序在运行时框架底层会自动生成这个接口的实现类对象

## IDEA 配置SQL提示
选择注解内的SQL语句，右键鼠标点击Show context action,选择Inject language or reference 选择MySQL语言

## JDBC介绍
- JDBC(Java DataBase Connectivity),就是使用Java语言操作关系型数据库的一套APl。

### 本质

- sun公司官方定义的一套操作所有关系型数据库的规范，即接口。
- 各个数据库厂商去实现这套接口，提供数据库驱动jar包。
- 我们可以使用这套接口(JDBC)编程，真正执行的代码是驱动jar包中的实现类。


## 数据库连接池
- 数据库连接池是个容器，负责分配、管理数据库连接(Connection)
- 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个
- 释放空闲时间超过最大空闲时间的连接，来避免因为没有释放连接而引起的数据库连接遗漏

### 优势
资源重用
提升系统响应速度
避免数据库连接遗漏


### 标准接口
标准接口:DataSource
- 官方(sun)提供的数据库连接池接口，由第三方组织实现此接口。
- 功能：获取连接 Connection getConnection() throws SQLException;

- 常见产品:
	- C3P0 
	- DBCP 
	- Druid
	- Hikari(Springboot默认)


- Druid（德鲁伊）
	- Druid连接池是阿里巴巴开源的数据库连接池项目功能强大，性能优秀，是Java语言最好的数据库连接池之一

## lombok
- Lombok是一个实用的Java类库，能通过注解的形式自动生成构造器、getter/setter、equals、hashcode、toString等方法，并可以自动化生成日志变量，简化Java开发、提高效率。

| 注解                   | 作用                                                                 |
|------------------------|----------------------------------------------------------------------|
| @Getter/@Setter         | 为所有的属性提供 get/set 方法                                         |
| @ToString               | 会给类自动生成易阅读的 toString 方法                                  |
| @EqualsAndHashCode      | 根据类所有的非静态字段自动重写 equals 方法和 hashCode 方法            |
| @Data                   | 提供了更综合的生成代码功能（@Getter + @Setter + @ToString + @EqualsAndHashCode） |
| @NoArgsConstructor      | 为实体类生成无参的构造器方法                                          |
| @AllArgsConstructor      | 为实体类生成除了 static 修饰的字段之外带有各参数的构造器方法          |

Lomboks会在编译时，自动生成对应的java代码。我们使用lombok时，还需要安装一个lombok的插件(idea自带)。

在 IntelliJ IDEA 中使用 Lombok，具体步骤如下：

### 1. 添加 Lombok 依赖
在 Maven 或 Gradle 项目中，首先需要在 `pom.xml` 或 `build.gradle` 文件中添加 Lombok 依赖。

**对于 Maven**，在 `pom.xml` 中添加以下依赖：
```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.28</version> <!-- 版本号可以根据需要更新 -->
    <scope>provided</scope>
</dependency>
```

**对于 Gradle**，在 `build.gradle` 文件中添加：
```gradle
dependencies {
    compileOnly 'org.projectlombok:lombok:1.18.28'
    annotationProcessor 'org.projectlombok:lombok:1.18.28'
}
```

### 2. 启用 Lombok 插件
1. 在 IntelliJ IDEA 的菜单栏中，点击 `File` -> `Settings` (Windows/Linux) 或 `IntelliJ IDEA` -> `Preferences` (Mac)。
2. 选择左侧菜单中的 `Plugins`，然后在插件搜索框中搜索 `Lombok`。
3. 安装 Lombok 插件后，点击 `Apply` 并重启 IDEA 以使其生效。

### 3. 启用注解处理器
1. 打开 `File` -> `Settings`。
2. 在左侧菜单中选择 `Build, Execution, Deployment` -> `Compiler` -> `Annotation Processors`。
3. 勾选 **"Enable annotation processing"** 选项，点击 `Apply` 保存设置。

### 4. 使用 Lombok 注解
在类中使用 Lombok 提供的注解，例如：
- `@Getter` 和 `@Setter`：自动生成 getter 和 setter 方法。
- `@Data`：包含 getter、setter、`toString`、`hashCode` 和 `equals` 方法。
- `@NoArgsConstructor` 和 `@AllArgsConstructor`：生成无参或全参构造器。

示例代码：
```java
import lombok.Data;

@Data
public class User {
    private String name;
    private int age;
}
```

