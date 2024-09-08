---
Project: 
Status:
tags: Resources
Deadline:
CreateTime:
Connected:
---


## 动态SQL
随着用户的输入或外部条件的变化而变化的SQL语句，我们称为动态SQL。

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">  
  
  
<mapper namespace="com.newboy.demo3.mapper.EmpMapper">  
    <sql id="commonSelete">  
        select * from emp  
    </sql>  
  
    <delete id="deleteByIds">  
        delete from emp where in  
        <foreach collection="ids" item="id" separator="," open="(" close=")">  
            #{id}  
        </foreach>  
    </delete>    <select id="list" resultType="com.newboy.demo3.pojo.Emp">  
        <include refid="commonSelete"></include>  
        <where>  
            <if test="name != null">  
                name like concat('%', #{name}, '%')  
            </if>  
            <if test="gender != null">  
                and gender = #{gender}  
            </if>  
  
            <if test="begin != null and end != null">  
                and entrydate between #{begin} and #{end}  
            </if>  
        </where>  
        order by update_time desc  
    </select>  
</mapper>

```

## if标签

用于判断条件是否成立。使用test属性进行条件判断，如果条件为true,则拼接SQL

```xml
<if test="name != null">
	name like concat('%',#{name},'%')
</if>
```


## where标签
where元素只会在子元素有内容的情况下才插入where子句。而且会自动去除子句的开头的AND或OR。

## set标签
动态地在行首插入SET关键字，并会删掉额外的逗号。(用在update语句中)

## foreach标签
### 属性
- collection: 集合名称
- item: 集合遍历出来的元素/项
- separator: 每一次遍历使用的分隔符
- open: 遍历开始前拼接的片段
- close: 遍历结束后拼接的片段

```xml
<delete id="deleteByIds">  
    delete from emp where in  
    <foreach collection="ids" item="id" separator="," open="(" close=")">  
        #{id}  
    </foreach>  
</delete>
```


## sql和include片段
```sql
<sql>:定义可重用的SQL片段。
<include>:通过属性refid,指定包含的sql片段。
```

```sql
<sql id="commonSelete">  
    select * from emp  
</sql>

<include refid="commonSelete"></include>
```