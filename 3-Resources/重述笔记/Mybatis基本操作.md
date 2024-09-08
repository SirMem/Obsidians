---
Project: "[[MybatisPlus]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-09-08
Connected:
---
## 删除
### 根据主键删除
mybatis参数占位符#{}

- SQL语句

```sql
delete from emp where id = #{id};
```

- 接口方法
```java
@Delete("delete from emp where id = #{id}")
public void delete(Integer id);
```

### 日志输出(预编译SQL)
- 可以在application.properties中，打开mybatis的日志，并指定输出到控制台。
```properties
#指定nybatis输出日志的位置，输出控制台
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

如果接口为delete，控制台可输出
```console
Preparing:delete from emp where id = ?   ##预编译SQL
Parameters:16(Integer)
```

#### 预编译SQL优势
- 性能更高

![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240908153527.png)

传统方式每次执行一条SQL语句时，MySQL服务器都要对SQL语句进行解析、优化和编译。而预编译后，SQL语句只需要执行一次解析、优化和编译过程，后续只需替换参数，大大减少了服务器的工作量。这是因为预编译的SQL语句可以被缓存，避免了多次执行相同类型的语句时的重复编译，进一步提升了执行速度。

预编译就像提前准备好了一份模板，只需要换上不同的变量值，而不用每次都从头写一份新模板，这样不仅节省时间，还减少了重复劳动。

- 更安全(防止SQL注入)

- SQL注入是通过操作输入的数据来修改事先定义好的SQL语句，以达到执行代码对服务器进行攻击的方法。

- 预编译的SQL语句防止了SQL注入攻击，因为参数的传递被严格限制为值，而不是直接拼接到SQL字符串中。

## 参数占位符

### #{...}
- 执行SQL时，会将#替换为?  ,生成预编译SQL,会自动设置参数值。
- 使用时机：参数传递，都使用#{...}

### ${...}
- 拼接SQL。直接将参数拼接在SQL语句中，存在SQL注入问题。
- 使用时机：如果对表名、列表进行动态设置时使用。


## 新增

### 1. SQL插入语句：
```sql
insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time)
values ('songyuanqiao', '宋远桥', 1, '1.jpg', 2, '2012-10-09', 2, '2022-10-01 10:00:00', '2022-10-01 10:00:00');
```

### 2. 接口方法（假设使用MyBatis框架）：
```java
@Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time) " +
        "values(#{username}, #{name}, #{gender}, #{image}, #{job}, #{entrydate}, #{deptId}, #{createTime}, #{updateTime})")
public void insert(Emp emp);
```

可以使用实现类对象传递数据
### 主键返回
描述：在数据添加成功后，需要获取插入数据库数据的主键。
如：添加套餐数据时，还需要维护套餐菜品关系表数据。

- 实现
```java
@Options(KeyProperty = "id", useGeneratedKeys = true) //自动将生成的主键值，赋值给emp对象的id属性
```


## 更新
### 接口方法(根据主键ID更新)
```java
@Update("update emp set username = #{username}, name = #{name}, gender = #{gender}, image = #{image}, job = #{job}, " +
        "entrydate = #{entrydate}, dept_id = #{deptId}, update_time = #{updateTime} where id = #{id}")
public void update(Emp emp);

```

可以使用实现类对象传递数据

## 查询
### 接口方法(根据主键ID更新)
```java
@Select("select * from emp where id = #{id}")  
public Emp getById(Integer id);
```

### 数据封装
- 实体类属性名和数据库表查询返回的字段名一致，mybatis会自动封装。
- 如果实体类属性名和数据库表查询返回的字段名不一致，不能自动封装。

### 避免实体类与数据库字段名不一致导致数据丢失
- 起别名
```java
@Select("select id, username, password, name, gender, image, job, entrydata" + " dept_id deptId, create_time createTime, update_time updateTime from emp where id = #{id}")  
public Emp getById(Integer id);
```

- 通过@Results, @Result注解手动映射封装
```java
@Results({
	@Result(column = "dept_id", property = "deptId"),
	@Result(column = "create_time", property = "createTime"),   //column对应的为字段名，property对应的为成员变量名
	@Result(column = "update_time", property = "updateTime"),
	
})
@Select("select * from emp where id = #{id}")  
public Emp getById(Integer id);
```

- 开启mybatis的驼峰命名自动映射开关
```properties
mybatis.configuration.map-underscore-to-camel-case=true //在application.properties添加驼峰映射
```


### concat 字符串拼接函数
- concat("String 1", "String 2")
```java
//条件查询  
@Select("select * from emp where name like concat('%', #{name}, '%') and gender = #{gender} and " +  
        "entrydate between #{begin} and #{end} order by update_time desc")  
public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);
```

### Param
@Param("name")String name 
早期对应的#{...}
"name" 对应 #{...}的注解