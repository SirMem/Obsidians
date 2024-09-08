---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-09-01
Connected:
---
## RESTful介绍

- RESTful是目前流行的互联网软件服务架构设计风格。
- REST(Representational State Transfer,表述性状态转移)一词是由Roy Thomas Fielding在2000年的博士论文中提出的，它定义了互联网软件服务的架构原则，如果一个架构符合REST原侧，则称之为RESTfu架构。
- REST并不是一个标准，它更像一组客户端和服务端交互时的架构理念和设计原则，基于这种架构理念和设计原则的Web APIE更加简洁，更有层次。

## RESTful的特点
- 每一个URI代表一种资源
- 客户端使用GET、POST、PUT、DELETE四种表示操作方式的动词对服务端资源进行操作：GET用于获取资源，POST用于新建资源（也可以用于更新资源）PUT用于更新资源，DELETE用于删除资源。
- 通过操作资源的表现形式来实现服务端请求操作。
- 资源的表现形式是JSON或者HTML。
- 客户端与服务端之间的交互在请求之间是无状态的，从客户端到服务端的每个请求都包含必需的信息。

## RESTful API
符合RESTful规范的Web APl需要具备如下两个关键特性：
- 安全性：安全的方法被期望不会产生任何副作用，当我们使用GET操作获取资源时，不会引起资源本身的改变，也不会引起服务器状态的改变。
- 幂等性：幂等的方法保证了重复进行一个请求和一次请求的效果相同（并不是指响应总是相同的，而是指服务器上资源的状态从第一次请求后就不再改变了），在数学上幂等性是指N次变换和一次变换相同。

## HTTP Method
- HTTP提供了POST、GET、PUT、DELETE等操作类型对某个Web资源进行Create、Read、Update和Delete操作。
- 一个HTTP请求除了利用URI标志目标资源之外，还需要通过HTTP Method指定针对该资源的操作类型，一些常见的HTTP方法及其在RESTfu风格下的使用：
- 参考图片![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240901101634.png)

## HTTP状态码
- HTTP状态码就是服务向用户返回的状态码和提示信息，客户端的每一次请求，服务都必须给出回应，回应包括HTTP状态码和数据两部分。
- HTTP定义了40个标准状态码，可用于传达客户端请求的结果。状态码分为以下5个类别：
	- 1xx:信息，通信传输协议级信息
	- 2xx:成功，表示客户端的请求已成功接受
	- 3xx:重定向，表示客户端必须执行一些其他操作才能完成其请求
	- 4xx:客户端错误，此类错误状态码指向客户端
	- 5xx:服务器错误，服务器负责这写错误状态码
- RESTful APl中使用HTTP状态码来表示请求执行结果的状态，适用于REST API设计的代码以及对应的HTTP方法。
![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240901102001.png)

## Spring Boot实现RESTful API
- Spring Boot提供的spring-boot-starter-web组件完全支持开发RESTful API,提供了与REST操作方式(GET、POST、PUT、DELETE)对应的注解。
	- @GetMapping:处理GET请求，获取资源。
	- @PostMapping:处理POST请求，新增资源。
	- @PutMapping:处理PUT请求，更新资源。
	- @DeleteMapping:处理DELETE请求，删除资源。
	- @PatchMapping:处理PATCH请求，用于部分更新资源。
```java
import org.springframework.web.bind.annotation.*;

@RestController
public class UserController {

    // 根据用户ID获取用户信息，@PathVariable获取动态路径
    @GetMapping("/user/{id}")
    public String getUserById(@PathVariable int id) {
        return "根据ID获取用户";
    }

    // 添加新用户
    @PostMapping("/user")
    public String save(User user) {
        return "添加用户";
    }

    // 更新用户信息
    @PutMapping("/user")
    public String update(User user) {
        return "更新用户";
    }

    // 根据用户ID删除用户
    @DeleteMapping("/user/{id}")
    public String deleteById(@PathVariable int id) {
        return "根据ID删除用户";
    }
}

```
- **路径变量**：在 `getUserById` 和 `deleteById` 方法中使用了路径变量 `{id}`，并通过 `@PathVariable` 注解与方法参数绑定。这使得可以在 URL 路径中直接传递用户 ID。
    
- **HTTP 方法映射**：每个方法通过不同的注解（如 `@GetMapping`、`@PostMapping`、`@PutMapping`、`@DeleteMapping`）映射到特定的 HTTP 方法，这种方式符合 RESTful API 设计的最佳实践。


- 在RESTful架构中，每个网址代表一种资源，所以URI中建议不要包含动词，只包含名词即可，而且所用的名词往往与数据库的表格名对应。
- 用户管理模块API示例：
![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240901102203.png)

## Swagger
### Swagger介绍
- Swagger是一个规范和完整的框架，用于生成、描述、调用和可视化RESTful风格的Web服务，是非常流行的API表达工具。
- Swagger能够自动生成完善的RESTful API文档，同时并根据后台代码的修改同步更新，同时提供完整的测试页面来调试API。

### 使用Swagger生成Web APl文档
- 在Spring BootI项目中集成Swaggerl同样非常简单，只需在项目中引入springfox-swagger2和springfox-swagger-ui依赖即可。
```Maven
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>

<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>

```

- 配置类
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration // 告诉Spring这是一个配置类
@EnableSwagger2 // 启用Swagger2功能
public class Swagger2Config {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo()) // 设置API信息
                .select() // 返回一个ApiSelectorBuilder实例来控制哪些接口暴露给Swagger
                .apis(RequestHandlerSelectors.basePackage("com")) // 扫描指定包下的API
                .paths(PathSelectors.any()) // 选择所有路径
                .build();
    }

    // API文档显示信息
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("示例项目API") // 设置标题
                .description("学习Swagger2的示例项目") // 设置描述
                .build();
    }
}

```

### 使用Swagger2进行接口测试
- 启动项目访问[http://127.0.0.1:8080/swagger-ui.html](http://127.0.0.1:8080/swagger-ui.html)，即可打开自动生成的可视化测试页面

### Swagger常用注解
- Swagger提供了一系列注解来描述接口信息，包括接口说明、请求方法、请求参数、返回信息等
![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240901104219.png)
