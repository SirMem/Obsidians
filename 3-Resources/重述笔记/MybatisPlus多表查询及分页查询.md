---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-09-02
Connected:
---

## 多表查询
- 实现复杂关系映射，可以使用@Resultsi注解，@Resulti注解，@One注解，@Many注解组合完成复杂关系的配置。

| 注解        | 说明                                                                                                                                                                |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @Results    | 代替`<resultMap>`标签，该注解可以加入单个或多个@Result注解                                                                                                             |
| @Result     | 代替`<id>`标签和`<Result>`标签，@Result中可以使用以下属性：<br>- column: 数据表的字段名称<br>- property: 类中对应的属性名<br>- one: 与@One注解联合，进行一对一的映射<br>- many: 与@Many注解联合，进行一对多的映射  |
| @One        | 代替`<association>`标签，用于指定查询中返回的一对一对象<br>通过`select`属性指定用于多表查询的方法<br>使用格式：@Result(column="", property="", one=@One(select=""))                                   |
| @Many       | 代替`<collection>`标签，用于指定查询中返回的集合对象<br>使用格式：@Result(column="", property="", many=@Many(select=""))                                              |

MyBatis 和 MyBatis-Plus 是流行的 Java 持久层框架，它们可以用于执行复杂的 SQL 查询。下面我将详细介绍如何使用 MyBatis 和 MyBatis-Plus 实现多条件查询、条件查询和分页查询。

### 一、MyBatis 教学

#### 1. 基本配置

在开始查询之前，确保已经配置好 MyBatis 环境。一般情况下，以下是必要的步骤：

- **创建数据库表**：确保数据库中有你要查询的表。
- **创建 MyBatis 配置文件**：`mybatis-config.xml`。
- **创建 Mapper 接口和 XML 映射文件**。

假设有一个用户表 `users`，结构如下：

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    age INT,
    email VARCHAR(50)
);
```

#### 2. 创建实体类

首先，为表创建一个实体类 `User`：

```java
public class User {
    private Integer id;
    private String name;
    private Integer age;
    private String email;

    // Getters and Setters
}
```

#### 3. 创建 Mapper 接口

创建一个 `UserMapper` 接口，用于定义 SQL 查询方法。

```java
public interface UserMapper {
    List<User> selectAllUsers();

    List<User> selectUsersByCondition(@Param("name") String name, @Param("age") Integer age);

    List<User> selectUsersByPage(@Param("offset") int offset, @Param("limit") int limit);
}
```

#### 4. 编写 XML 映射文件

创建 `UserMapper.xml` 文件来定义 SQL 语句。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.UserMapper">

    <!-- 查询所有用户 -->
    <select id="selectAllUsers" resultType="User">
        SELECT * FROM users
    </select>

    <!-- 多条件查询 -->
    <select id="selectUsersByCondition" parameterType="map" resultType="User">
        SELECT * FROM users
        WHERE 1=1
        <if test="name != null and name != ''">
            AND name = #{name}
        </if>
        <if test="age != null">
            AND age = #{age}
        </if>
    </select>

    <!-- 分页查询 -->
    <select id="selectUsersByPage" parameterType="map" resultType="User">
        SELECT * FROM users
        LIMIT #{offset}, #{limit}
    </select>

</mapper>
```

#### 5. MyBatis 的多条件查询

使用 `selectUsersByCondition` 方法来实现多条件查询。

```java
public class MyBatisExample {
    public static void main(String[] args) {
        SqlSessionFactory sqlSessionFactory = MyBatisUtil.getSqlSessionFactory();
        try (SqlSession session = sqlSessionFactory.openSession()) {
            UserMapper userMapper = session.getMapper(UserMapper.class);

            // 条件查询
            List<User> users = userMapper.selectUsersByCondition("Alice", 25);
            users.forEach(System.out::println);
        }
    }
}
```

#### 6. MyBatis 的分页查询

使用 `selectUsersByPage` 方法来实现分页查询。

```java
public class MyBatisExample {
    public static void main(String[] args) {
        SqlSessionFactory sqlSessionFactory = MyBatisUtil.getSqlSessionFactory();
        try (SqlSession session = sqlSessionFactory.openSession()) {
            UserMapper userMapper = session.getMapper(UserMapper.class);

            // 分页查询
            int page = 1;
            int pageSize = 10;
            int offset = (page - 1) * pageSize;

            List<User> users = userMapper.selectUsersByPage(offset, pageSize);
            users.forEach(System.out::println);
        }
    }
}
```

### 二、MyBatis-Plus 教学

MyBatis-Plus 是在 MyBatis 的基础上扩展的增强工具，它提供了丰富的 API，可以更简便地实现常见的操作。

#### 1. 基本配置

- 添加 MyBatis-Plus 依赖。

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.3</version>
</dependency>
```

- 配置 `application.yml` 或 `application.properties` 文件。

```yaml
mybatis-plus:
  mapper-locations: classpath:/mapper/*.xml
  type-aliases-package: com.example.entity
```

#### 2. 创建实体类

实体类 `User` 和 MyBatis 的示例相同，但需要添加 MyBatis-Plus 的注解。

```java
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.Data;

@Data
@TableName("users")
public class User {
    @TableId
    private Integer id;
    private String name;
    private Integer age;
    private String email;
}
```

#### 3. 创建 Mapper 接口

MyBatis-Plus 的 Mapper 接口需要继承 `BaseMapper<T>`。

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;

public interface UserMapper extends BaseMapper<User> {
}
```

#### 4. 多条件查询

MyBatis-Plus 提供了 `QueryWrapper` 类来构建查询条件。

```java
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public List<User> getUsersByCondition(String name, Integer age) {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        if (name != null && !name.isEmpty()) {
            queryWrapper.eq("name", name);
        }
        if (age != null) {
            queryWrapper.eq("age", age);
        }
        return userMapper.selectList(queryWrapper);
    }
}
```

#### 5. 分页查询

MyBatis-Plus 提供了 `Page` 类来实现分页查询。

```java
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public Page<User> getUsersByPage(int page, int pageSize) {
        Page<User> userPage = new Page<>(page, pageSize);
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        return userMapper.selectPage(userPage, queryWrapper);
    }
}
```

#### 6. 分页插件配置

确保在 Spring Boot 项目中配置分页插件：

```java
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MyBatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return interceptor;
    }
}
```

### 总结

- **多条件查询**：使用 `QueryWrapper` 或 MyBatis 自定义 XML 中的 `<if>` 标签来实现。
- **分页查询**：MyBatis-Plus 提供了内置的分页支持，通过 `Page<T>` 类和 `PaginationInnerInterceptor` 插件实现。
- **条件查询**：可以通过构造动态 SQL 或使用 `QueryWrapper` 对象来实现。

这些步骤可以帮助你实现 MyBatis 和 MyBatis-Plus 的多条件查询、条件查询以及分页查询。