---
Project: "[[MybatisPlus]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 
Connected:
---


## 查询user表中所有数据
1. 创建user表，添加数据
2. 创建模块，导入坐标
3. 编写MyBatis核心配置文件->替换连接信息解决硬编码问题
4. 编写SQL映射文件->统一管理sql语句，解决硬编码问题
5. 编码
	1. 定义POJO类
	2. 加载核心配置文件，获取SqlSessionFactory对象
	3 .获取SqlSession对象，执行SQL语句
	4. 释放资源
