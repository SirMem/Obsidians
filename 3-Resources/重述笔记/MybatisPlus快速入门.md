---
Project: "[[MybatisPlus]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-09-01
Connected:
---

## ORM介绍
- ORM (Object Relational Mapping,对象关系映射)是为了解决面向对象与关系数据库存在的互不匹配现象的一种技术。
- ORM通过使用描述对象和数据库之间映射的元数据将程序中的对象自动持久化到关系数据库中。
- ORM框架的本质是简化编程中操作数据库的编码。
![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240901135032.png)

## MyBatis-Plus介绍
- MyBatis是一款优秀的数据持久层ORM框架，被广泛地应用于应用系统。
- MyBatis能够非常灵活地实现动态SQL,可以使用XML或注解来配置和映射原生信息，能够轻松地将Java的POJO(Plain Ordinary Java Object,普通的Java对象）与数据库中的表和字段进行映射关联。
- MyBatis-Plus是一个MyBatis的增强工具，在MyBatis的基础上做了增强，简化了开发。

## IDEA添加依赖
```xml
<!-- MyBatisPlus 依赖 -->  
<dependency>  
    <groupId>com.baomidou</groupId>  
    <artifactId>mybatis-plus-spring-boot3-starter</artifactId>  
    <version>3.5.7</version> 
</dependency>  
  
<!-- MySQL 驱动依赖 -->  
<dependency>  
    <groupId>mysql</groupId>  
    <artifactId>mysql-connector-java</artifactId>  
    <version>8.0.32</version>  
</dependency>  
  
<!-- 数据连接池 druid --><dependency>  
    <groupId>com.alibaba</groupId>  
    <artifactId>druid</artifactId>  
    <version>1.2.6</version>  
</dependency>

```

## 全局配置
### 配置数据库相关信息
```java
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mydb?useSSL=false  //Mybatis固定端口3306
spring.datasource.username=root
spring.datasource.password=root
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl

```

### 添加@MapperScan注解
```java
@SpringBootApplication
@MapperScan("com.xx.mapper")
public class MybatisPlusDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(MybatisPlusDemoApplication.class, args);
    }
}

```

## Mybatis CRUD注解
| 注解       | 功能                       |
| -------- | ------------------------ |
| @Insert  | 实现插入                     |
| @Update  | 实现更新                     |
| @Delete  | 实现删除                     |
| @Select  | 实现查询                     |
| @Result  | 实现结果集封装                  |
| @Results | 可以与 @Result 一起使用，封装多个结果集 |
| @One     | 实现一对一结果集封装               |
| @Many    | 实现一对多结果集封装               |

## CRUD 操作
```java
@Mapper
public interface UserMapper {

    @Insert("insert into user values(#{id}, #{username}, #{password}, #{birthday})")
    int add(User user);

    @Update("update user set username=#{username}, password=#{password}, birthday=#{birthday} where id=#{id}")
    int update(User user);

    @Delete("delete from user where id=#{id}")
    int delete(int id);

    @Select("select * from user where id=#{id}")
    User findById(int id);

    @Select("select * from user")
    List<User> getAll();
}

```

## BaseMapper类继承

### **前提条件**

1. **环境配置**：确保项目中已经集成了 MyBatis-Plus。通常你需要在 `pom.xml` 中添加 MyBatis-Plus 的依赖（对于 Maven 项目）：

   ```xml
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-boot-starter</artifactId>
       <version>3.5.3.1</version> <!-- 确保版本号是最新的稳定版本 -->
   </dependency>
   ```

2. **数据库配置**：在 `application.yml` 或 `application.properties` 文件中配置数据库连接信息，如下：

   ```yaml
   spring:
     datasource:
       url: jdbc:mysql://localhost:3306/your_database_name?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
       username: your_username
       password: your_password
       driver-class-name: com.mysql.cj.jdbc.Driver
   ```

3. **开启 MyBatis-Plus 支持**：确保在 Spring Boot 应用程序的启动类上添加了 `@MapperScan` 注解，指定 Mapper 接口所在的包：

   ```java
   import org.mybatis.spring.annotation.MapperScan;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   @MapperScan("com.example.mapper") // 替换为你实际的包名
   public class MyApplication {
       public static void main(String[] args) {
           SpringApplication.run(MyApplication.class, args);
       }
   }
   ```

### **使用 `BaseMapper` 类的步骤**

1. **创建实体类**：创建一个实体类，映射数据库表。例如，一个简单的 `User` 实体类：

   ```java
   package com.example.entity;

   import com.baomidou.mybatisplus.annotation.TableId;
   import com.baomidou.mybatisplus.annotation.TableName;

   @TableName("user") // 指定数据库表名
   public class User {
       @TableId // 指定主键
       private Long id;
       private String username;
       private String email;

       // 省略 getter 和 setter 方法
   }
   ```

2. **创建 `UserMapper` 接口**：创建一个 Mapper 接口，并继承 `BaseMapper` 接口，指定实体类的类型。使用 `@Mapper` 注解来标识它是一个 Mapper 组件：

   ```java
   package com.example.mapper;

   import com.baomidou.mybatisplus.core.mapper.BaseMapper;
   import com.example.entity.User;
   import org.apache.ibatis.annotations.Mapper;

   @Mapper
   public interface UserMapper extends BaseMapper<User> {
       // 这里可以定义自定义的 SQL 方法
   }
   ```

   - `BaseMapper<User>` 提供了对 `User` 实体类基本的 CRUD 操作，如插入、删除、更新和查询等。
   - `@Mapper` 注解是为了将该接口标识为 MyBatis 的 Mapper，通常在使用 `@MapperScan` 时不需要单独添加该注解，但为了保持明确性和兼容性，还是推荐加上。

3. **使用 `UserMapper` 接口**：在 Service 层或其他业务逻辑中注入并使用 `UserMapper`。例如：

   ```java
   package com.example.service;

   import com.example.entity.User;
   import com.example.mapper.UserMapper;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;

   import java.util.List;

   @Service
   public class UserService {

       @Autowired
       private UserMapper userMapper;

       public List<User> getAllUsers() {
           return userMapper.selectList(null); // 查询所有用户
       }

       public User getUserById(Long id) {
           return userMapper.selectById(id); // 根据 ID 查询用户
       }

       public void addUser(User user) {
           userMapper.insert(user); // 插入新用户
       }

       public void deleteUser(Long id) {
           userMapper.deleteById(id); // 删除用户
       }
   }
   ```

### **BaseMapper 提供的常用方法**

`BaseMapper` 接口提供了一些通用的 CRUD 方法，可以直接使用，无需额外编写 SQL：

|方法名|返回类型|描述|
|---|---|---|
|`int insert(T entity)`|`int`|插入一条记录。返回受影响的行数。|
|`int deleteById(Serializable id)`|`int`|根据 ID 删除一条记录。返回受影响的行数。|
|`int deleteByMap(Map<String, Object> columnMap)`|`int`|根据 `columnMap` 条件删除记录。返回受影响的行数。|
|`int delete(Wrapper<T> wrapper)`|`int`|根据条件删除记录。返回受影响的行数。|
|`int updateById(T entity)`|`int`|根据 ID 更新记录，返回受影响的行数。|
|`int update(T entity, Wrapper<T> updateWrapper)`|`int`|根据条件更新记录，返回受影响的行数。|
|`T selectById(Serializable id)`|`T`|根据 ID 查询一条记录。|
|`List<T> selectBatchIds(Collection<? extends Serializable> idList)`|`List<T>`|根据 ID 集合查询多条记录。|
|`List<T> selectByMap(Map<String, Object> columnMap)`|`List<T>`|根据 `columnMap` 条件查询多条记录。|
|`T selectOne(Wrapper<T> queryWrapper)`|`T`|根据条件查询一条记录，如果结果超过一条会抛出异常。|
|`Integer selectCount(Wrapper<T> queryWrapper)`|`Integer`|根据条件查询记录数。|
|`List<T> selectList(Wrapper<T> queryWrapper)`|`List<T>`|根据条件查询多条记录。|
|`List<Map<String, Object>> selectMaps(Wrapper<T> queryWrapper)`|`List<Map<String, Object>>`|根据条件查询多条记录，并返回一个 `Map` 列表。|
|`List<Object> selectObjs(Wrapper<T> queryWrapper)`|`List<Object>`|根据条件查询多条记录，只返回指定列的值列表。|
|`IPage<T> selectPage(IPage<T> page, Wrapper<T> queryWrapper)`|`IPage<T>`|根据条件查询并分页返回记录。|
|`IPage<Map<String, Object>> selectMapsPage(IPage<T> page, Wrapper<T> queryWrapper)`|`IPage<Map<String, Object>>`|根据条件分页查询，并返回 `Map` 列表。|

### **总结**

- **`@Mapper` 注解**：标识一个接口是 MyBatis 的 Mapper 接口，用于自动生成代理对象。使用 `@MapperScan` 时，可以省略 `@Mapper` 注解，但推荐保留以增加代码的可读性和兼容性。
- **`BaseMapper` 类**：提供了基本的 CRUD 操作，继承它的接口无需编写 SQL 语句就可以执行基本的数据库操作。
- **Service 层**：通过注入 Mapper 接口来调用数据库操作方法，保持代码的层次结构清晰。
``