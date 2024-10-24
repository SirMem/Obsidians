---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-09-14
Connected:
  - "[[SQL索引和事务]]"
  - Java异常
---
## Springboot事务管理
### 简介

- 注解：@Transactional
- 位置：业务(service)层的方法上、类上、接口上
- 作用：将当前方法交给spring进行事务管理，方法执行前，开启事务；成功执行完毕，提交事务；出现异常，回滚事务

```yml
#开启事务管理日志
logging:
	level:
		org.springframework.jdbc.support.jdbcTransactionManager: debug
```

## 事务属性-回滚
### rollbackfor
默认情况下，只有出现RuntimeException才回滚异常 rollbackFor属性用于控制出现何种异常类型，回滚事务。

```java
@Transactional(rollbackFor = Exception.class)
@Override
public void delete(Integer id) throws Exception {
    deptMapper.deleteById(id); // 删除部门
    if (true) {
        throw new Exception("出错信息...");
    }
    empMapper.deleteByDeptId(id); // 删除部门下的员工
}

```

## 事务属性-传播行为
propagation
事务传播行为：指的就是当一个事务方法被另一个事务方法调用时，这个事务方法应该如何进行事务控制。
```java
// A方法
@Transactional
public void a(){
	userService.b();
	//......
}

//B方法 
@Transactional
public void b(){
	//......
}


//A方法调用了B方法，但二者均有事务，B方法被调用也会开启事务吗，如何进行控制
```

```java
//事务控制注解,Required为默认值
@Transactional(propagation = Propagation.REQUIRED) 
```

### 事务属性值表
| 属性值           | 含义                                  |
| ------------- | ----------------------------------- |
| REQUIRED      | [默认值] 需要事务，有则加入，无则创建新事务             |
| REQUIRES_NEW  | 需要新事务，无论有无，总是创建新事务，并在运行完毕后，执行挂起的父事务 |
| SUPPORTS      | 支持事务，有则加入，无则在无事务状态中运行               |
| NOT_SUPPORTED | 不支持事务，在无事务状态下运行，如果当前存在已有事务，则挂起当前事务  |
| MANDATORY     | 必须有事务，否则抛异常                         |
| NEVER         | 必须没有事务，否则抛异常                        |
### 案例:解散部门时，记录操作日志
需求：解散部门时，无论是成功还是失败，都要记录操作日志。
步骤：
- 解散部门：删除部门、删除部门下的员工
- 记录日志到数据库表中

```java
//执行删除操作记录日志的service方法
@Transactional
@Override
public void delete(Integer id) throws Exception {
    try {
        deptMapper.deleteById(id); // 根据ID删除部门数据

        int i = 1/0;
        // if(true) { throw new Exception("出错啦..."); }

        empMapper.deleteByDeptId(id); // 根据部门ID删除该部门下的员工
    } finally {
        DeptLog deptLog = new DeptLog();
        deptLog.setCreateTime(LocalDateTime.now());
        deptLog.setDescription("执行了解散部门的操作，此次解散的是" + id + "号部门");
        deptLogService.insert(deptLog); //调用将日志插入进数据库的service方法
    }
}

// insert操作的service方法
@Transactional(propagation = Propagation.REQUIREED_NEW) //Propagation属性为REQUIRED就会导致加入到父事务中，从而导致父事务发生异常无法调用子事务的方法
@Override
public void insert(DeptLog deptLog){
	deptLogMapper.insert(deptLog);
}

```

### 场景
REQUIRED：大部分情况下都是用该传播行为即可。
REQUIRES_NEW：当我们不希望事务之间相互影响时，可以使用该传播行为。比如：下订单前需要记录日志，不论订单保存成功与否，都需要保证日志记录能够记录成功。