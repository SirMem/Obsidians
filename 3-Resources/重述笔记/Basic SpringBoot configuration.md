---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-08-14
Connected:
---
## SpringBoot特点
1. 遵循“约定优于配置”的原侧，只需要很少的配置或使用默认的配置。
2. 能够使用内嵌的Tomcat、Jetty服务器，不需要部署war文件。
3. 提供定制化的启动器Starters,简化Maven配置，开箱即用。
4. 纯Java配置，没有代码生成，也不需要XML配置。
5. 提供了生产级的服务监控方案，如安全监控、应用监控、健康检测等。

## IDEA 组件依赖需求
1. Spring Web


## IDEA 组件配置
### Maven
Setting-> Build Tools -> Maven -> modify path of maven repository and settings.xml

pom.xel是Maven的配置文件

## 开发环境热部署
### 背景
在实际的项目开发调试过程中会频繁地修改后台类文件，导致需要重新编译，重新启动，整个过程非常麻烦，影响开发效率。

Spring Boot提供了spring-boot-devtools组件，使得无须手动重启Spring Boot应用即可重新编译、启动项目，大大缩短编译启动的时间。

devtools会监听classpath下的文件变动，触发Restart类加载器重新加载该类从而实现类文件和属性文件的热部署。

并不是所有的更改都需要重启应用（如静态资源、视图模板），可以通过设置spring.devtools..restart.exclude属性来指定一些文件或目录的修改不用重启应用

### 导入
在pom.xml配置文件中添加dev-tools依赖

使用optional=true表示依赖不会传递，即该项目依赖devtools;其他项目如果引入此项目生成的JAR包，则不会包含devtools

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
	<optional>true</optional>
</dependency>
```

### 部署
在application.properties中配置devtools.

```java
#热部署生效
spring.devtools.restart.enabled=true

#设置重启目录
spring.devtools.restart.additional-paths=src/main/java

#设置classpath目录下WEB-INF文件夹内容修改不重启
spring.devtools.restart.exclude=static/**
```

打开Settings -> Build -> Compile 勾选Build  project automatically