---
Project: "[[MybatisPlus]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-09-02
Connected:
---
## 常用注解
从图片中提取的内容如下：

### 代码部分

```java
@TableName("tb_user")
public class User {
    @TableId(value = "id", type = IdType.AUTO)
    private Long id;

    @TableField("username")
    private String name;

    @TableField("is_married")
    private Boolean isMarried;

    @TableField("order")
    private Integer order;

    @TableField(exist = false)
    private String address;
}
```

### 文字内容

**IdType枚举：**

- `AUTO`：数据库自增
- `INPUT`：通过 set 方法自行输入
- `ASSIGN_ID`：分配 ID，接口 `IdentifierGenerator` 的方法 `nextId` 生成 id，默认实现类为 `DefaultIdentifierGenerator`（雪花算法）

**使用 `@TableField` 的常见场景：**

- 成员变量名与数据库字段名不一致
- 成员变量名以 `is` 开头，且是布尔值
- 成员变量名与数据库关键字冲突
- 成员变量不是数据库字段


## 常用配置
```java
mybatis-plus:
  type-aliases-package: com.itheima.mp.domain.po  # 别名扫描包
  mapper-locations: "classpath*:/mapper/**/*.xml"  # Mapper.xml 文件地址，默认值
  configuration:
    map-underscore-to-camel-case: true  # 是否开启下划线和驼峰的映射
    cache-enabled: false  # 是否开启二级缓存
  global-config:
    db-config:
      id-type: assign_id  # id 为雪花算法生成
      update-strategy: not_null  # 更新策略：只更新非空字段

```