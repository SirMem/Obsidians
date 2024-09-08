---
Project: "[[MybatisPlus]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-09-08
Connected:
---

## XML映射文件
### 规范
- XML映射文件的名称与Mapper接口名称一致，并且将XML映射文件和Mapper接口放置在相同包下（同包同名）。
- XML映射文件的namespace属性为Mapper接口全限定名一致。
- XML映射文件中sql语句的id与Mapper接口中的方法名一致，并保持返回类型一致。

[Mybatis约束配置网站](https://mybatis.org/mybatis-3/getting-started.html#exploring-mapped-sql-statements)

文件头约束配置
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
```

![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240908170225.png)

示例文件1
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">  
  
<mapper namespace="com.newboy.demo3.mapper.EmpMapper">  
    <select id="list" resultType="com.newboy.demo3.pojo.Emp">  
        select * from emp where name like concat('%', #{name}, '%') and gender = #{gender} and  
        entrydate between #{begin} and #{end} order by update_time desc  
    </select>  
</mapper>
```


## MybatisX
MybatisX插件: 帮助快速从Mapper接口定位到xml的sql语句

使用Mybatis的注解，主要是来完成一些简单的增删改查功能。如果需要实现复杂的SQL功能，建议使用XML来配置映射语句。