---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-08-31
Connected:
---

## 静态资源访问
- 使用IDEA创建Spring Boot项目，会默认创建出classpath:/static/目录，静态资源一般放在这个目录下即可。
- 如果默认的静态资源过滤策略不能满足开发需求，也可以自定义静态资源过滤策略。

```java
在application.properties中直接定义过滤规则和静态资源位置：
spring.web.resources.static-locations=classpath:/static/
spring.mvc.static-path-pattern=/static/**
过滤规则为/static/*,静态资源位置为classpath:/static/
```

- 在典型的 Spring Boot 项目中，开发者会把静态资源（例如图片、CSS、JS 文件）放在 `src/main/resources/static` 目录中。
- 当项目构建时（例如使用 Maven 或 Gradle 构建时），构建工具会将 `src/main/resources` 目录下的所有内容复制到 `target/classes` 目录中。这就是为什么你会在 `target/classes/static` 中找到这些资源文件的原因。

## 文件上传原理
- 表单的enctype属性规定在发送到服务器之前应该如何对表单数据进行编码
- 当表单的enctype:="application/,x-www-form-urlencoded"（默认）时，form表单中的数据格式为：key=value&key=value
- 当表单的enctype:="multipart/form-data"时，其传输数据形式如下
- ![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240831200753.png)

## SpringBoot
- Spring Boot.工程嵌入的tomcat限制了请求的文件大小，每个文件的配置最大为1Mb,单次请求的文件的总数不能大于10Mb.
- 要更改这个默认值需要在配置文件(如application.properties)中加入两个配置
```java
spring.servlet.multipart.max-file-size=10MB  //每个文件请求
spring.servlet.multipart.max-request-size=10MB //单次请求的所有文件
```


- 当表单的enctype="multipart/form-data"时，可以使用MultipartFile获取上传的文件数据，再通过transferTo方法将其写入到磁盘中
```java
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

import javax.servlet.http.HttpServletRequest;
import java.io.File;
import java.io.IOException;

@RestController
public class FileController {

    @PostMapping("/upload")
    public String upload(String nickname, MultipartFile photo, HttpServletRequest request) throws IOException {
        // 打印昵称
        System.out.println(nickname);

        // 获取图片的原始名称
        System.out.println(photo.getOriginalFilename());

        // 获取文件类型
        System.out.println(photo.getContentType());

        // 获取服务器上的实际路径，将文件保存到项目的 upload 目录下
        String path = request.getServletContext().getRealPath("/upload/");
        System.out.println(path);

        // 保存文件
        saveFile(photo, path);

        return "上传成功";
    }

    public void saveFile(MultipartFile photo, String path) throws IOException {
        // 判断存储的目录是否存在，如果不存在则创建
        File dir = new File(path);
        if (!dir.exists()) {
            dir.mkdir();  // 创建目录
        }

        // 创建保存文件的路径和文件名
        File file = new File(path + photo.getOriginalFilename());

        // 将上传的文件保存到目标文件中
        photo.transferTo(file);
    }
}

```

### 代码解析

1. **`upload` 方法**:
    
    - 接收一个字符串 `nickname` 和一个 `MultipartFile` 对象 `photo`（上传的文件），以及 `HttpServletRequest` 对象 `request`。
    - 使用 `request.getServletContext().getRealPath("/upload/")` 方法获取服务器上的真实路径，构成文件上传目录的路径。这个路径是基于当前 Web 应用的根目录的 `/upload/` 文件夹。
    - 调用 `saveFile(photo, path)` 方法，将文件保存到指定的路径中。
2. **`saveFile` 方法**:
    
    - 检查文件存储目录是否存在，如果不存在，则使用 `mkdir()` 方法创建该目录。
    - 使用 `photo.getOriginalFilename()` 获取上传文件的原始文件名，并创建 `File` 对象指向该路径。
    - 使用 `photo.transferTo(file)` 将上传的文件保存到指定路径。

### 使用说明

- 这种方式利用 `request.getServletContext().getRealPath("/upload/")` 来动态获取服务器上的存储路径，因此无论在开发环境还是生产环境中，上传的文件都可以被正确地存储在应用的 `upload` 目录下。
- 该代码会在项目的部署目录下创建一个 `upload` 文件夹，并将上传的文件保存到这个目录中。
- 在实际使用中，可以通过前端表单提供 `nickname` 和文件上传输入域来调用这个上传功能。

## 拦截器
### 概念
- 拦截器在Web系统中非常常见，对于某些全局统一的操作，我们可以把它提取到拦截器中实现。总结起来，拦截器大致有以下几种使用场景：
	- 权限检查：如登录检测，进入处理程序检测是否登录，如果没有，则直接返回登录页面。
	- 性能监控：有时系统在某段时间莫名其妙很慢，可以通过拦截器在进入处理程序之前记录开始时间，在处理完后记录结束时间，从而得到该请求的处理时间
	- 通用行为：读取cookie得到用户信息并将用户对象放入请求，从而方便后续流程使用，还有提取Locale、Theme信息等，只要是多个处理程序都需要的，即可使用拦截器实现。

- Spring Boot定义了HandlerInterceptor接口来实现自定义拦截器的功能
- Handlerlnterceptor接口定义了preHandle、.postHandle、afterCompletion三种方法，通过重写这三种方法实现请求前、请求后等操作
- ![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240831210231.png)
在 Spring MVC 中，`HandlerInterceptor` 接口用于对 HTTP 请求进行拦截处理，它定义了三个方法：`preHandle`、`postHandle` 和 `afterCompletion`。通过重写这些方法，可以实现请求处理前后和请求完成后的逻辑操作。

### `HandlerInterceptor` 接口三种方法及其用途
- Spring Boot使用拦截器需要需要创建一个拦截器类，这个类需要实现 `HandlerInterceptor` 接口，通常我们只需重写其中的一些方法（如 `preHandle`、`postHandle`、`afterCompletion`）。
```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

public class MyInterceptor implements HandlerInterceptor {

    // 在请求处理之前调用
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("Pre Handle method is Calling");
        // 返回 true 表示继续流程（如调用下一个拦截器或处理器）
        // 返回 false 表示流程终止（如不会调用其他拦截器或处理器）
        return true;
    }

    // 在请求处理之后调用，但在视图渲染之前
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("Post Handle method is Calling");
    }

    // 在整个请求结束之后调用，通常用于资源清理
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception exception) throws Exception {
        System.out.println("Request and Response is completed");
    }
}

```
1. **`preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)`**:
   - **用途**: 该方法在请求处理之前被调用。可以用来做一些前置的操作，比如验证用户身份、检查请求参数、记录日志、权限检查等。
   - **返回值**: 返回 `boolean` 类型。如果返回 `true`，表示继续执行后续的拦截器和请求处理器；如果返回 `false`，表示请求到此为止，不会继续调用其他的拦截器和处理器，通常可以在这里通过 `response` 返回错误信息。
   - **最常用且重要**: `preHandle` 是 `HandlerInterceptor` 接口中最常用且重要的方法。它在实际开发中通常用于安全检查和请求预处理，如用户认证、IP 限制等。这些检查通常是拦截请求的第一步，如果检查未通过，可以立即终止请求。

2. **`postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)`**:
   - **用途**: 该方法在请求处理之后、视图渲染之前被调用。可以用来对请求处理的结果进行修改或添加额外的信息，比如向模型中添加公共数据，调整视图或模型属性等。
   - **常用于**: 动态修改视图数据、统一处理响应信息、记录操作日志等。
   - **注意**: 如果 `preHandle` 方法返回 `false`，则 `postHandle` 不会被调用。

3. **`afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)`**:
   - **用途**: 该方法在请求处理完成后，即视图渲染完成后执行。常用于清理资源（如关闭流）、记录日志（记录操作完成情况）、异常处理等。
   - **常用于**: 确保资源的正确释放，不论请求是成功还是失败，特别是用来处理一些异常情况。
   - **调用顺序**: 无论 `preHandle` 返回 `true` 还是 `false`，该方法都会被调用。它通常用于所有请求的最终处理和清理操作。

### 方法的调用顺序

1. **`preHandle`**: 首先执行。如果返回 `false`，整个请求处理链终止。
2. **`postHandle`**: 在 `preHandle` 返回 `true` 并且请求处理成功后执行，在视图渲染之前执行。
3. **`afterCompletion`**: 在整个请求结束之后，无论请求是否成功，都会执行。这个方法用来进行资源清理和后续操作。

### 举例说明处理流程

假设我们有一个拦截器 `MyInterceptor`，并且在配置中添加了这个拦截器：

1. 客户端发送请求到服务器。
2. `MyInterceptor.preHandle` 方法首先被调用：
   - 例如，检查用户是否已登录。如果返回 `false`，请求会被拦截，结束流程；否则继续。
3. 请求被处理器（如控制器）处理。
4. `MyInterceptor.postHandle` 方法在处理器处理后调用：
   - 可以在这里修改 `ModelAndView`，比如添加一些公共的数据。
5. 视图被渲染（如果有）。
6. `MyInterceptor.afterCompletion` 方法在视图渲染后调用：
   - 可以在这里进行资源清理、日志记录等操作。

### 总结

- **`preHandle` 方法最常用且最重要**，因为它用于处理请求前的逻辑，是安全验证和请求预处理的关键点。
- **`postHandle`** 和 **`afterCompletion`** 则主要用于请求后的操作，如数据修改、日志记录和资源清理等。

## 拦截器注册
- 为了让 Spring Boot知道我们定义的拦截器并应用它，我们需要配置拦截器的注册。通常，我们在配置类中实现 `WebMvcConfigurer` 接口的`addInterceptors` 方法来注册拦截器。
```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 注册自定义拦截器
        registry.addInterceptor(new MyInterceptor())
                .addPathPatterns("/**")  // 拦截所有请求
                .excludePathPatterns("/login", "/register");  // 排除特定路径不拦截
    }
}

```
- addPathPatterns方法定义拦截的地址
- excludePathPatterns定义排除某些地址不被拦截
- 添加的一个拦截器没有addPathPattern任何一个url则默认拦截所有请求
- 如果没有excludePathPatterns任何一个请求，则默认不放过任何一个请求。

### 完整项目中如何使用

通常，`WebConfig` 类会放在一个专门的配置包（如 `com.example.config`）下，并且会被 Spring 自动扫描。`@Configuration` 注解表明这是一个配置类，它会被 Spring 上下文自动加载。

### 多个拦截器的注册

如果需要注册多个拦截器，可以在 `addInterceptors` 方法中多次调用 `registry.addInterceptor()` 方法：
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new MyInterceptor())
                .addPathPatterns("/**")
                .excludePathPatterns("/login", "/register");
        
        registry.addInterceptor(new AnotherInterceptor())
                .addPathPatterns("/admin/**");  // 只拦截 /admin 下的所有请求
    }
}

```