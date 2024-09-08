---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 
Connected:
---

## SpringBoot Web入门
- Spring Boot将传统Web开发的mvc、json、tomcat等框架整合，提供了spring-boot-starter-web组件，简化了Web应用配置。
- 创建Spring Boot项目勾选Spring Web选项后，会自动将spring-boot-starter-web组件加入到项目中。
- spring-boot-starter-web启动器主要包括web、webmvc、.json、tomcat等基础依赖组件，作用是提供Wb开发场景所需的所有底层依赖。
- webmvc为Web开发的基础框架，json为SON数据解析组件，tomcat为自带的容器依赖。

## 控制器
### 介绍
- Spring Boot提供了@Controller和@RestControllerp两种注解来标识此类负责接收和处理HTTP请求。
- 如果请求的是页面和数据，使用@Controlleri注解即可；如果只是请求数据，则可以使用@RestControlleri注解。
- ![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240828155035.png)

### Controller
```java
@Controller
public class HelloController{
	@RequestMapping("/hello")
	public String index(ModelMap map){
		map.addAttribute("name", "zhangsan");
		return "hello";
	}
}
```

- 示例中返回了hello页面和name的数据，在前端页面中可以通过${name参数获取后台返回的数据并显示。
- Controlleri通常与Thymeleaft模板引擎结合使用。

### *RestController*
```java
@RestController
public class HelloController{

	@RequestMapping("/user")
	public User getUser(){
		User user = new User();
		user.setUsername("zhangsan");
		user.setPassword("123");
		return user;
	}
}
```
- 默认情况下，@RestController注解会将返回的对像数据转换为JSON格式。

## 控制器的路由映射
### @RequestMapping的路由映射
- @RequestMapping注解主要负责URL的路由映射。它可以添加在Controller类或者具体的方法上。
- 如果添加在Controller类上，则这个Controller中的所有路由映射都将会加上此映射规则，如果添加在方法上，则只对当前方法生效。
- @RequestMapping注解包含很多属性参数来定义HTTP的请求映射规则。

### @RequestMapping的属性参数
- value:请求URL的路径，支持URL模板、正则表达式
- method:HTTP请求方法
- consumes:请求的媒体类型(Content-Type),如application/json
- produces:响应的媒体类型
- params,headers:请求的参数及请求头的值

通常consumes，produces，params和headers参数很少涉及

### value
```java
- @RequestMapping的value属性用于匹配URL映射，value支持简单表达式@RequestMapping("/user)

- @RequestMapping?支持使用通配符匹配URL,用于统一映射某些URL规则类似的请求：@RequestMapping('/getJson/*json"),当在浏览器中请求/getJson/a.json或者/getJson/b.json时都会匹配到后台的Json方法

- @RequestMapping的通配符匹配非常简单实用，支持"*”“？”“**”等通配符符号“*”匹配任意字符，符号“”匹配任意路径，符号“？”匹配单个字符有通配符的优先级低于没有通配符的，比如/user/add,json比/user/json优先匹配。

- 有"**"通配符的优先级低于有"*"通配符的
```

## Method 匹配
```java
HTTP请求Method有GET、POST、PUT、DELETE等方式。HTTP支持的全部Method

@RequestMapping注解提供了method参数指定请求的Method类型，包括
RequestMethod.GET、RequestMethod.POST、RequestMethod.DELETE、
RequestMethod.PUT等值，分别对应HTTP请求的Method

example:
@RequestMapping(value = "/getData", method = RequestMethod.GET);
public String getData(){
	return "hello";
}

Method匹配也可以使用@GetMapping、@PostMapping等注解代替。
```

## 方法的参数映射
```java
@RequestMapping(value = "/hello2", method = RequestMethod.GET)  
public String hello2(@RequestParam("name") String nickname, String phone) {  
    System.out.println("nickname: " + nickname);  
    return "Hello, World!" + nickname + phone;  
}
```

@RequestParam可以将内容映射到变量中,切必须要传递内容的参数

## Postman模拟多个请求
- **点击“New”按钮** 或者 **选择“File” > “New Tab”**，这将打开一个新的请求标签页。
- 选择请求类型：在请求输入框左侧有一个下拉菜单，您可以选择请求的类型，例如 `GET`、`POST`、`PUT`、`DELETE` 等。
- 在请求 URL 输入框中输入您的本地服务器地址。例如，如果服务器运行在 `localhost` 的端口 `8080` 上，并且您想访问 `/api/data` 路径，那么输入：http://localhost:8080/api/data

## 使用@RequestBody接受Post请求的Json信息
在 Spring Boot 中，使用 `@RequestBody` 注解可以轻松地接收 POST 请求中的 JSON 格式信息，并将其绑定到 Java 对象上。下面是一个详细的步骤说明，展示如何在 Spring Boot 应用程序中使用 `@RequestBody` 接收 POST 请求的 JSON 数据。

### 1. 创建一个 Java 类来表示 JSON 数据

首先，创建一个 POJO 类（Plain Old Java Object），用于映射 JSON 数据。例如，我们假设我们想接收一个用户的基本信息（如名字和邮箱），可以创建一个 `User` 类：

```java
package com.example.demo.model;

public class User {
    private String name;
    private String email;

    // Getters and Setters
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    // Optional: toString method for easier debugging
    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}
```

### 2. 创建一个 Spring Boot 控制器类

接下来，创建一个控制器类，定义一个接收 POST 请求的方法，并使用 `@RequestBody` 注解来绑定请求中的 JSON 数据到 `User` 对象上。

```java
package com.example.demo.controller;

import com.example.demo.model.User;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @PostMapping("/api/users")
    public String createUser(@RequestBody User user) {
        // 打印接收到的用户信息，实际应用中可以进行其他处理
        System.out.println("Received user: " + user);

        // 返回一个简单的响应
        return "User is created with name: " + user.getName() + " and email: " + user.getEmail();
    }
}
```

### 3. 配置 Spring Boot 应用程序

- 确保你的 Spring Boot 项目有以下依赖（通常在 `pom.xml` 文件中）。这些依赖通常默认在 `spring-boot-starter-web` 中：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

- 确保 Spring Boot 应用程序主类（即包含 `main` 方法的类）上有 `@SpringBootApplication` 注解。

### 4. 运行 Spring Boot 应用程序

运行你的 Spring Boot 应用程序（例如，通过使用 IDE 的运行按钮，或者在命令行中使用 `mvn spring-boot:run`）。

### 5. 使用 Postman 测试 POST 请求

- 打开 Postman 并创建一个新的 POST 请求。
- 设置请求 URL 为：`http://localhost:8080/api/users`（默认端口是 8080）。
- 在 `Body` 标签页中选择 `raw`，并将格式设置为 `JSON`。
- 输入 JSON 数据，例如：

    ```json
    {
        "name": "John Doe",
        "email": "john.doe@example.com"
    }
    ```

- 点击 `Send` 按钮发送请求。

### 6. 查看响应

在 Postman 中，你应该看到服务器返回的响应，例如：

```
User is created with name: John Doe and email: john.doe@example.com
```

同时，在 Spring Boot 控制台中，你应该看到打印的用户信息：

```
Received user: User{name='John Doe', email='john.doe@example.com'}
```

### 结论

通过使用 `@RequestBody` 注解，Spring Boot 可以轻松地将 POST 请求中的 JSON 数据映射到 Java 对象。这种方法适用于创建 RESTful API，处理复杂的 JSON 数据输入。


## 统一相应结果
是的，在SpringBoot项目中，通常会创建一个统一的响应结果类（如`Result`类），以标准化API接口的返回格式。通过定义`code`, `msg`, 和 `data`三个字段，可以使不同接口的返回数据保持一致。典型的`Result`类如下：

```java
public class Result {
    private Integer code;   // 状态码
    private String msg;     // 返回信息
    private Object data;    // 返回数据

    // 构造函数
    public Result() {}

    public Result(Integer code, String msg, Object data) {
        this.code = code;
        this.msg = msg;
        this.data = data;
    }

    // 静态方法: 成功时返回
    public static Result success(Object data) {
        return new Result(200, "成功", data);
    }

    // 静态方法: 出错时返回
    public static Result error(String msg) {
        return new Result(500, msg, null);
    }

    // Getter 和 Setter 方法
    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }
}
```

在控制器中，可以像这样使用`Result`类统一响应数据：

```java
@RestController
public class ExampleController {

    @GetMapping("/example")
    public Result example() {
        // 处理逻辑...
        return Result.success("这是成功的响应数据");
    }

    @GetMapping("/errorExample")
    public Result errorExample() {
        // 处理错误逻辑...
        return Result.error("这是错误的响应消息");
    }
}
```

这样，你的所有接口返回的结果都将是统一的格式，包括状态码、信息和数据。