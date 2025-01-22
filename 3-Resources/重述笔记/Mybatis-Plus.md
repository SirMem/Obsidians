---
Project: "[[数据库]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-10-31
Connected:
---

#review

# 快速入门
## BaseMapper
支持单表增删改查方法

## 约定
类名驼峰转下划线作为表名
名为id的字段作为主键
变量名驼峰转下刘线作为表的字段名


简单来说就是约定大于配置
只要你遵循了约定呢
那mp就能利用反射得到实体类的信息

## 常见注解
@TableName 指定表名
@TableId 用来指定表中的主键字段信息
@TableField 用来指定表中的普通字段信息

IdType枚举：
AUTO:数据库自增长
INPUT:通过set方法自行输入
ASSIGN ID:分配lD,接口IdentifierGenerator的方法nextld来生成id
默认实现类为DefaultldentifierGenerator 雪花算法
使用@TableField的常见场景：
成员变量名与数据库字段名不一致
成员变量名以is开头,且是布尔值
成员变量名与数据库关键字冲突
成员变量不是数据库字段

## 常见配置
那如果说呢你的SQL语句啊比较复杂
或者说呢你是多表的这种查询
其实呢你还是需要去手写SQL语句的
那因此呢唉他这里也允许你去指定这个
允许你去自定义SQL好

```yml
mybatis-plus:
type-aliases-package:com.itheima.mp.domain.po#别名扫描包
mapper-locations:"classpath*:/mapper/**/*.xml"#Mapper,xml文件地t址，默认值
configuration:
map-underscore-to-camel-case:true#是否开启下划线和驼峰的映射
cache-enabled:false#是否开启二级缓存
global-config:
db-config:
id-type:assign_id#id为雪花算法生成
update-strategy:not_null#更新策略：只更新非空字段
```

# 核心功能
## 条件构造器
QueryWrapper

```java
QueryWrapper<User> wrapper = new QueryWrapper<User>()
	.select("id", "username", "info", "banlance") 
	.like("username", "o")
	.ge("banlance", 1000);

List<User> users = userMapper.selectList(wrapper);
```

UpdateWrapper

基于lambda的删除条件构造器

那为什么要用lambda的语法呢

这是因为啊大家看啊
我们现在在使用啊这个query rapper的时候
你看我们这个字段名是不是写死的
特别是上面这个方法
你看所有的字段是直接写死的
那么这种写法呢其实就属于是硬编码了
这其实在开发规约当中啊
是不太那推荐我们这么去写
那么lambda的这种rapper就是来解决这个问题
lambda的函数参数可以通过反射的形式得到column

```java
LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<User>()
	.select(User::getId, User::getUsername, User::getInfo, User::getBalance) 
	.like(User::getUsername, "o")
	.ge( User::getBalance, 1000);
```
LambdaQueryWrapper

条件构造器的用法：
·QueryWrappera和LambdaQueryWrapper通常用来构建select
公
、delete、update的where条件部分
·UpdateWrapper和LambdaUpdateWrapper通常只有在set语
句比较特殊才使用
·尽量使用LambdaQueryWrappera和LambdaUpdateWrapper
，避免硬编码

## 自定义SQL
为什么不使用自定义SQL就不符合开发规范

利用MyBatisPlus的Wrapper来构建复杂的Where条件，然后自己定义SQL语句中剩下的部分

![[Pasted image 20241031205024.png]]

## IService
相比于Mapper，多了批处理的功能

MP的Service接口使用流程是怎样的？
自定义Service接口继承IService接口
自定义Service实现类，实现自定义接口并继承Servicelmpl类

### 基本接口

### 复杂接口