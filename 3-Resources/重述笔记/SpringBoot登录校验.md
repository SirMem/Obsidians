---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 
Connected:
---

## 无状态协议
无状态协议是指**比如客户获得一张网页之后关闭浏览器，然后再一次启动浏览器，再登录该网站，但是服务器并不知道客户关闭了一次浏览器**。

## 会话技术
- 浏览器和服务器端的一次连接为一次会话
- 会话：用户打开浏览器，访问Wb服务器的资源，会话建立，直到有一方断开连接，会话结束。在一次会话中可以包含多次请求和响应。
- 会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间共享数据，

- 会话跟踪方案：
	- 客户端会话跟踪技术：Cookie
	- 服务端会话跟踪技术：Session
	- 令牌技术

![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240912151340.png)
cookie是在cookie里面存用户信息，session是在cookie里面存sessionid，拿到id后去服务端解析出来用户信息


### Session优缺点
- 优点: 存储在服务器端，安全
- 确定:服务器集群环境下无法直接使用Session
### 令牌优缺点
- 优点: 
	- 支持PC、移动端
	- 解决集群环境下的认证问题
	- 减轻服务器的存储压力
- 缺点:
	- 难以实现

HttpServletResponse
## JWT令牌
### 简介
全称：JSON Web Token (https://jwt.io/)

- 定义了一种简洁的、自包含的格式，用于在通信双方以JSON数据格式安全的传输信息。由于数字签名的存在，这些信息是可靠的。

- 组成：
  - 第一部分：Header(头)，记录令牌类型、签名算法等。例如：`{"alg":"HS256","type":"JWT"}`
  - 第二部分：Payload(有效载荷)，携带一些自定义信息，默认信息等。例如：`{"id":"1","username":"Tom"}`
  - 第三部分：Signature(签名)，防止Token被篡改，确保安全性。将header、payload，并加入指定密钥，通过指定签名算法计算而来。

- eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.JTdCJTIybmFtZSUyMiUzQSUyRlRvbSUyMiUyQyUyMlhdCUyMiQTE1MYYkJzIiNQ9.= SflKxwRJSMeKKF2QT4fwpMeJf...
**Base64**：是一种基于64个可打印字符（A-Z a-z 0-9 + /）来表示二进制数据的编码方式。

### 登录认证场景
登录成功后，生成令牌
后续每个请求，都要携带JWT令牌，系统在每次请求处理之前，进行校验令牌，通常才能处理请求

### JWT-生成
#### 引入依赖
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>

```
#### 调用API
```java
@Test
public void genJwt() {
    Map<String, Object> claims = new HashMap<>();
    claims.put("id", 1);
    claims.put("username", "Tom");

    String jwt = Jwts.builder()
        .setClaims(claims) // 自定义内容(载荷)
        .signWith(SignatureAlgorithm.HS256, "itheima") // 签名算法
        .setExpiration(new Date(System.currentTimeMillis() + 12 * 3600 * 1000)) // 有效期
        .compact();

    System.out.println(jwt);
}

```

```java
// 校验JWT

```java
@Test
public void parseJwt() {
    Claims claims = Jwts.parser()
        .setSigningKey("itheima") // 指定签名秘钥
        .parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjU5OTk1NTE3LCJ1c2VybmFtZSI6IlRvbSJ9.EUTfeqPkGsIekdKBezcWCe7a7xbclIwB1MXllccTMwo") // 解析令牌
        .getBody();
    System.out.println(claims);
}
```
```

#### 注意事项
JWT校验时使用的签名秘钥，必须和生成WT令牌时使用的秘钥是配套的。
如果JWT令牌解析校验时报错，则说明WT令牌被篡改或失效了，令牌非法。
```


## 过滤器Filter
- 概念：Filter过滤器，是JavaWeb三大组件(Servlet、Filter、Listener)之一。
- 过滤器可以把对资源的请求拦截下来，从而实现一些特殊的功能。
- 过滤器一般完成一些通用的操作，比如：登录校验、统一编码处理、敏感字符处理等。


### 快速入门
1.定义Filter:定义一个类，实现Filter接▣，并重写其所有方法。
2.配置Filter:Filter类上加@WebFilter注解，配置拦截资源的路径。引导类上加@ServletComponentScan开启Servlet组件支持。

```java
//Filter定义
@WebFilter(urlPatterns = "/*")

public class DemoFilter implements Filter {
	@Override
    public void init(FilterConfig filterConfig) throws ServletException { 
        // 初始化方法，Web服务器启动，创建Filter时调用，只调用一次
        Filter.super.init(filterConfig); 
    }
	@Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
        // 拦截到请求时，调用该方法，可调用多次
        // 放行前操作
        System.out.println("拦截方法执行，拦截到了请求..."); 
        //放行
        chain.doFilter(request, response);
        //放行后操作
    }
	@Override
    public void destroy() { 
        // 销毁方法，服务器关闭时调用，只调用一次
        Filter.super.destroy(); 
    }
}

```


```java
//Controller使用
@ServletComponentScan //添加@ServletComponetScan注解
@SpringBootApplication
public class TliasWebManagementApplication {
    public static void main(String[] args) {
        SpringApplication.run(TliasWebManagementApplication.class, args);
    }
}

```

### Filter拦截路径
|拦截路径|urlPatterns值|含义|
|---|---|---|
|拦截具体路径|`/login`|只有访问 `/login` 路径时，才会被拦截|
|目录拦截|`/emps/*`|访问 `/emps/` 下的所有资源，都会被拦截|
|拦截所有|`/*`|访问所有资源，都会被拦截|
### 过滤器链
介绍：一个web应用中，可以配置多个过滤器，这多个过滤器就形成了一个过滤器链。
顺序：注解配置的Filter,优先级是按照过滤器类名（字符串）的自然排序。

### 登录校验
- 所有的请求，拦截到了之后，都需要校验令牌吗？
- 有一个例外，登录请求
- 拦截到请求后，什么情况下才可以放行，执行业务操作？
- 有令牌，且令牌校验通过（合法）；否则都返回未登录错误结果

```mermaid
graph TD
    A(请求) --> B(获取请求路径)
    B --> C{判断是否是登录请求}
    C -->|是| E(放行)
    C -->|否| D(获取请求头token)
    D --> F{判断是否有token}
    F -->|否| G(返回未登录结果)
    F -->|是| H(解析token)
    H --> I{解析成功吗?}
    I -->|成功| E(放行)
    I -->|失败| G(返回未登录结果)

```

#### 步骤
获取请求url
判断请求url中是否包含login,如果包含，说明是登录操作，放行。
获取请求头中的令牌(token).
判断令牌是否存在，如果不存在，返回错误结果（未登录）。
解析token,如果解析失败，返回错误结果（未登录）。
放行。
#### 代码实现
```java
@Slf4j
//@WebFilter(urlPatterns = "/*")
public class LoginCheckFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest req = (HttpServletRequest) request;
        HttpServletResponse resp = (HttpServletResponse) response;

        //1.获取请求url。
        String url = req.getRequestURL().toString();
        log.info("请求的url: {}",url);

        //2.判断请求url中是否包含login，如果包含，说明是登录操作，放行。
        if(url.contains("login")){
            log.info("登录操作, 放行...");
            chain.doFilter(request,response);
            return;
        }

        //3.获取请求头中的令牌（token）。
        String jwt = req.getHeader("token");

        //4.判断令牌是否存在，如果不存在，返回错误结果（未登录）。
        if(!StringUtils.hasLength(jwt)){
            log.info("请求头token为空,返回未登录的信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换 对象--json --------> 阿里巴巴fastJSON
            String notLogin = JSONObject.toJSONString(error);
            resp.getWriter().write(notLogin);
            return;
        }

        //5.解析token，如果解析失败，返回错误结果（未登录）。
        try {
            JwtUtils.parseJWT(jwt);
        } catch (Exception e) {//jwt解析失败
            e.printStackTrace();
            log.info("解析令牌失败, 返回未登录错误信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换 对象--json --------> 阿里巴巴fastJSON
            String notLogin = JSONObject.toJSONString(error);
            resp.getWriter().write(notLogin);
            return;
        }

        //6.放行。
        log.info("令牌合法, 放行");
        chain.doFilter(request, response);

    }
}

```


## 拦截器Interceptor
概念：是一种动态拦截方法调用的机制，类似于过滤器。Spring框架中提供的，用来动态拦截控制器方法的执行。
作用：拦截请求，在指定的方法调用前后，根据业务需要执行预先设定的代码。
Ctrl + O
### 快速入门
- 定义拦截器，实现HandlerInterceptor接口，并重写其所有方法。

```java
@Component
public class LoginCheckInterceptor implements HandlerInterceptor {

    @Override //目标资源方法执行前执行，放回true:放行，返回false:不放行
    public boolean preHandle(HttpServletRequest req, HttpServletResponse resp, Object handler) throws Exception {
        System.out.println("preHandle ...");
        return true;
    }

    @Override //目标资源方法执行后执行
    public void postHandle(HttpServletRequest req, HttpServletResponse resp, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle ...");
    }

    @Override //视图渲染完毕后执行，最后执行
    public void afterCompletion(HttpServletRequest req, HttpServletResponse resp, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion ...");
    }
}

```
- 注册拦截器
```java
//配置类Configuration 
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginCheckInterceptor).addPathPatterns("/**");
    }
}

```

### 拦截路径
```java
@Override
public void addInterceptors(InterceptorRegistry registry){
	registry.addInterceptor(loginCheckInterceptor).addPathPatterns ("/**").excludePathPatterns("/login");

//addPathPatterns ("/**")添加拦截路径
//excludePathPatterns("/login") 排除拦截拦截
}
```


| 拦截路径      | 含义           | 举例                                                         |
| ------------- | -------------- | ------------------------------------------------------------ |
| `/*`          | 一级路径       | 能匹配 `/depts`, `/emps`, `/login`，不能匹配 `/depts/1`       |
| `/**`         | 任意级路径     | 能匹配 `/depts`, `/depts/1`, `/depts/1/2`                    |
| `/depts/*`    | `/depts` 下的一级路径 | 能匹配 `/depts/1`，不能匹配 `/depts/1/2`, `/depts`             |
| `/depts/**`   | `/depts` 下的任意级路径 | 能匹配 `/depts`, `/depts/1`, `/depts/1/2`，不能匹配 `/emps/1` |

### 执行流程
![image.png](https://raw.githubusercontent.com/SirMem/PicGo/main/img/20240913094843.png)

接口规范不同：过滤器需要实现Filter接口，而拦截器需要实现Handlerlnterceptor接口
拦截范围不同：过滤器Filter会拦截所有的资源，而Interceptor只会拦截Spring环境中的资源。

### 登录校验
和过滤器登录校验逻辑类似
```java
//过滤器类
@Slf4j
@Component
public class LoginCheckInterceptor implements HandlerInterceptor {
    @Override //目标资源方法运行前运行, 返回true: 放行, 放回false, 不放行
    public boolean preHandle(HttpServletRequest req, HttpServletResponse resp, Object handler) throws Exception {
        //1.获取请求url。
        String url = req.getRequestURL().toString();
        log.info("请求的url: {}",url);

        //2.判断请求url中是否包含login，如果包含，说明是登录操作，放行。
        if(url.contains("login")){
            log.info("登录操作, 放行...");
            return true;
        }

        //3.获取请求头中的令牌（token）。
        String jwt = req.getHeader("token");

        //4.判断令牌是否存在，如果不存在，返回错误结果（未登录）。
        if(!StringUtils.hasLength(jwt)){
            log.info("请求头token为空,返回未登录的信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换 对象--json --------> 阿里巴巴fastJSON
            String notLogin = JSONObject.toJSONString(error);
            resp.getWriter().write(notLogin);
            return false;
        }

        //5.解析token，如果解析失败，返回错误结果（未登录）。
        try {
            JwtUtils.parseJWT(jwt);
        } catch (Exception e) {//jwt解析失败
            e.printStackTrace();
            log.info("解析令牌失败, 返回未登录错误信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换 对象--json --------> 阿里巴巴fastJSON
            String notLogin = JSONObject.toJSONString(error);
            resp.getWriter().write(notLogin);
            return false;
        }

        //6.放行。
        log.info("令牌合法, 放行");
        return true;
    }

    @Override //目标资源方法运行后运行
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle ...");
    }

    @Override //视图渲染完毕后运行, 最后运行
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...");
    }
}

```


## 异常处理
全局异常处理器

```mermaid
graph TD
    A[Controller] --> B[Service]
    B --> C[Mapper]
    C --> B
    B --> A
    A --> D[全局异常处理器]
    D --> E[/External System/]
```


```java
//异常类
//@RestControllerAdvice=  @ControllerAdvice +  @ResponseBody
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public Result ex(Exception ex) {
        ex.printStackTrace();
        return Result.error("对不起，操作失败，请联系管理员");
    }
}

```

## JWT
# JWT入门基础概念

## 1.什么是JWT

[JWT官方地址](https://jwt.io/introduction/)

json web token（JWT）是一个开放标准（rfc7519），它定义了一种紧凑的、自包含的方式，用于在各方之间以JSON对象安全地传输信息。它是以JSON形式作为Web应用中的令牌,用于在各方之间安全地将信息作为JSON对象传输。在数据传输过程中还可以完成数据加密、签名等相关处理。

## 2.JWT能做什么

**授权**

这是使用JWT的最常见方案。**一旦用户登录，每个后续请求将包括JWT**，从而允许用户访问该令牌允许的路由，服务和资源。单点登录是当今广泛使用JWT的一项功能，因为它的开销很小并且可以在不同的域中轻松使用。

**信息交换**

JSON Web Token是在各方之间安全地传输信息的好方法。因为可以对JWT进行签名（例如，使用公钥/私钥对），所以您可以确保发件人是他们所说的人。此外，由于签名是使用标头和有效负载计算的，因此您还可验证内容是否遭到篡改。

## 3.Session认证与JWT认证的区别

### 基于传统的Session认证策略

#### 1.认证方式

`http协议本身是一种无状态的协议`，而这就意味着如果用户向我们的应用提供了用户名和密码来进行用户认证，那么下一次请求时，用户还要再一次进行用户认证才行。

因为根据http协议，我们并不能知道是哪个用户发出的请求，所以为了让我们的应用能识别是哪个用户发出的请求，我们只能在服务器存储一份用户登录的信息，**这份登录信息（sessionId）会在响应时传递给浏览器**，告诉其保存为cookie,以便下次请求时发送给我们的应用，这样我们的应用就能识别请求来自哪个用户了,这就是传统的基于session认证。

#### 2.认证流程

![image-20200726103959013](images/imagesimage-20200726103959013.png)

#### 3.Session认证的问题

- 每个用户经过我们的应用认证之后，我们的应用都要在服务端做一次记录，以方便用户下次请求的鉴别，通常而言session都是保存在内存中，而随着认证用户的增多，服务端的开销会明显增大

- 用户认证之后，服务端做认证记录，如果认证的记录被保存在内存中的话，这意味着用户下次请求还必须要请求在这台服务器上,这样才能拿到授权的资源，这样在分布式的应用上，相应的限制了负载均衡器的能力。这也意味着限制了应用的扩展能力。

- 因为是基于cookie来进行用户识别的, cookie如果被截获，用户就会很容易受到跨站请求伪造的攻击。

- 在前后端分离系统中就更加痛苦！也就是说前后端分离在应用解耦后增加了部署的复杂性。
  - **通常用户一次请求就要转发多次。如果用session 每次携带sessionid 到服务器，服务器还要查询用户信息。**同时如果用户很多。这些信息存储在服务器内存中，给服务器增加负担。
  - CSRF（跨站伪造请求攻击）攻击，session是基于cookie进行用户识别的, cookie如果被截获，用户就会很容易受到跨站请求伪造的攻击。
  - sessionid 就是一个特征值，表达的信息不够丰富。不容易扩展。而且如果你后端应用是多节点部署。那么就需要实现session共享机制。	不方便集群应用。

### 基于JWT认证的策略

#### 1.认证流程

- 首先，前端通过Web表单将自己的用户名和密码发送到后端的接口。这一过程一般是一个HTTP POST请求。建议的方式是通过SSL加密的传输（https协议），从而避免敏感信息被嗅探。

- 后端核对用户名和密码成功后，**将用户的id等其他信息作为JWT Payload（负载），将其与头部分别进行Base64编码拼接后签名，形成一个JWT(Token)。形成的JWT就是一个形同xxx.yyy.zzz的字符串**。 

  `即token =  head.payload.singurater`

- **后端将JWT字符串作为登录成功的返回结果返回给前端**。前端可以将返回的结果保存在localStorage或sessionStorage上，退出登录时前端删除保存的JWT即可。

- **前端在每次请求时将JWT放入HTTP Header中的Authorization位**。(解决XSS和XSRF问题) HEADER

- **后端检查是否存在，如存在验证JWT的有效性**。例如，检查签名是否正确；检查Token是否过期；检查Token的接收方是否是自己（可选）。

- 验证通过后后端使用JWT中包含的用户信息进行其他逻辑操作，返回相应结果。

![image-20200726183248298](images/imagesimage-20200726183248298.png)

#### 2.jwt优势

- 简洁(Compact): 可以通过URL，POST参数或者在HTTP header发送，因为数据量小，传输速度也很快

- 自包含(Self-contained)：负载中包含了所有用户所需要的信息，避免了多次查询数据库

- 因为Token是以JSON加密的形式保存在客户端的，所以JWT是跨语言的，原则上任何web形式都支持。

- 不需要在服务端保存会话信息，特别适用于分布式微服务。

## 4.JWT的结构

### 令牌组成

- 1.标头(Header)
- 2.有效载荷(Payload)
- 3.签名(Signature)
> 因此，JWT通常如下所示:xxxxx.yyyyy.zzzzz 也就是 Header.Payload.Signature

### Header的组成信息

- **标头通常由两部分组成：**`令牌的类型（即JWT）和所使用的签名算法`，例如HMAC SHA256或RSA。它会使用 Base64 编码组成 JWT 结构的第一部分。

- 注意:Base64是一种编码，也就是说，它是可以被翻译回原来的样子来的。它并不是一种加密过程。

```json
# header的组成信息
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### Payload组成信息

令牌的第二部分是有效负载，其中包含声明。声明是有关实体（通常是用户信息）和其他数据的声明。同样的，它会使用 Base64 编码组成 JWT 结构的第二部分

```json
# payload组成信息
{
  "id": "823",
  "name": "Code Duck",
  "role": "admin"
}
```

### Signature的组成信息

`header和payload`都是使用 Base64 进行编码的，即前端可以解开知道里面的信息。**Signature 需要使用编码后的 header 和 payload 以及`我们自己的一个密钥`，然后使用 header 中指定的签名算法（HS256）进行签名。**签名的作用是保证 JWT 没有被篡改过。

> **例如：HMACSHA256( base64UrlEncode(header) + "." + base64UrlEncode(payload), secret );**

**签名目的**

- 最后一步签名的过程，实际上是对头部以及负载内容进行签名，防止内容被窜改。如果有人对头部以及负载的内容解码之后进行修改，再进行编码，最后加上之前的签名组合形成新的JWT的话，那么服务器端会判断出新的头部和负载形成的签名和JWT附带上的签名是不一样的。如果要对新的头部和负载进行签名，在不知道服务器加密时用的密钥的话，得出来的签名也是不一样的。

**信息安全问题**

- 在这里大家一定会问一个问题：Base64是一种编码，是可逆的，那么我的信息不就被暴露了吗？

- 是的。所以，在JWT中，不应该在负载里面加入任何敏感的数据。在上面的例子中，我们传输的是用户的User ID。这个值实际上不是什么敏感内容，一般情况下被知道也是安全的。但是像密码这样的内容就不能被放在JWT中了。如果将用户的密码放在了JWT中，那么怀有恶意的第三方通过Base64解码就能很快地知道你的密码了。因此JWT适合用于向Web应用传递一些非敏感信息。JWT还经常用于设计用户认证和授权系统，甚至实现Web应用的单点登录。

### 以上三部分进行整合

**JWT的真实面目：**

`(Header)`eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.

`(payLoad)`eyJyb2xlIjoiZW1wbG95ZWUiLCJpZCI6IjQiLCJleHAiOjE1OTcxMjc3NjUsInVzZXJuYW1lIjoiamFzb24ifQ.

`(Signature)`WxIiTf7V4UaboMONu0UpPu-uQSuDQFZqepKKxLstnaU

**:point_down:形象解释**

![image-20200726181136113](images/imagesimage-20200726181136113.png)

- JWT令牌是三个由点分隔的Base64-URL字符串，可以在HTML和HTTP环境中轻松传递这些字符串，与基于XML的标准（例如SAML）相比，它更紧凑。
- 简洁(Compact)，可以通过URL, POST 参数或者在 HTTP header 发送，因为数据量小，传输速度快
- 自包含(Self-contained)，负载中包含了所有用户所需要的信息，避免了多次查询数据库

# Springboot整合JWT

### 1.创建Springboot项目

### 2.配置pom信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.1.RELEASE</version>
        <relativePath/>
    </parent>

    <groupId>com.duck.code</groupId>
    <artifactId>Springboot-JWT</artifactId>
    <version>1.0-SNAPSHOT</version>
    
    <dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!--引入jwt-->
        <dependency>
            <groupId>com.auth0</groupId>
            <artifactId>java-jwt</artifactId>
            <version>3.4.0</version>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!--mybatis plus-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.3.2</version>
        </dependency>

        <!--代码生成器-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.3.2</version>
        </dependency>

        <!--逆向工程需要模板引擎-->
        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>2.3.30</version>
        </dependency>

        <!--mysql依赖-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!--连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.20</version>
        </dependency>
    </dependencies>
</project>
```

### 3.配置application配置文件信息

```yml
server:
  port: 8081
spring:
  datasource:
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db_jwt?useUnicode=true&useSSL=false&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    type: com.alibaba.druid.pool.DruidDataSource

mybatis-plus:
  global-config:
    db-config:
      table-prefix: tb_   # 表明前缀
  type-aliases-package: com.code.duck.entity  # 实体类所在包
  mapper-locations: classpath*:/mapper/**Mapper.xml   # xml文件所在位置
```

### 5.Springboot主启动类配置

```java
@SpringBootApplication
@MapperScan("com.duck.code.mapper")
public class JWTApplication {
    public static void main(String[] args) {
        SpringApplication.run(JWTApplication.class,args);
    }
}
```

### 4.Springboot整合MybatisPlus

​	使用MybatisPlus代码生成器，生成entity、controller、service，service.impl、mapper、xml文件

​	代码生成器参考本人：Springboot整合MybatisPlus的文章https://www.cnblogs.com/code-duck/p/13481550.html

### 6.封装JWT工具类

> JWTUtil工具类主要用于生成JWT令牌（token）、核实token信息、对token进行解密

#### JWT抽象类

**JWT抽象类中的三个方法，对应于上述三个功能**

```java
public abstract class JWT {

    /**
     * Decode a given Json Web Token.
     *
     * @param token with jwt format as string.
     * @return a decoded JWT.
     */
    public static DecodedJWT decode(String token) throws JWTDecodeException {
        return new JWTDecoder(token);
    }

    /**
     * Returns a {@link JWTVerifier} builder with the algorithm to be used to validate token signature.
     *
     * @param algorithm that will be used to verify the token's signature.
     * @return {@link JWTVerifier} builder
     * @throws IllegalArgumentException if the provided algorithm is null.
     */
    public static Verification require(Algorithm algorithm) {
        return JWTVerifier.init(algorithm);
    }

    /**
     * Returns a Json Web Token builder used to create and sign tokens
     *
     * @return a token builder.
     */
    public static JWTCreator.Builder create() {
        return JWTCreator.init();
    }
}
```

#### JWTUtil工具类

```java
public class JWTUtil {

    // 用于JWT进行签名加密的秘钥
    private static String SECRET = "code-duck-*%#@*!&";

    /**
     * @Param: 传入需要设置的payload信息
     * @return: 返回token
     */
    public static String generateToken(Map<String, String> map) {
        JWTCreator.Builder builder = JWT.create();

        // 将map内的信息传入JWT的payload中
        map.forEach((k, v) -> {
            builder.withClaim(k, v);
        });

        // 设置JWT令牌的过期时间为60
        Calendar instance = Calendar.getInstance();
        instance.add(Calendar.SECOND, 60);
        builder.withExpiresAt(instance.getTime());

        // 设置签名并返回token
        return builder.sign(Algorithm.HMAC256(SECRET)).toString();
    }

    /**
     * @Param: 传入token
     * @return:
     */
    public static void verify(String token) {
        JWT.require(Algorithm.HMAC256(SECRET)).build().verify(token);
    }

    /**
     * @Param: 传入token
     * @return: 解密的token信息
     */
    public static DecodedJWT getTokenInfo(String token) {
        return JWT.require(Algorithm.HMAC256(SECRET)).build().verify(token);
    }
}
```

### 7.UserController控制器中声明登录接口

```java
@RestController
@Slf4j
public class UserController {

    @Resource
    private UserService userService;

    @PostMapping(value = "/user/login")
    public Map<String, String> userLogin(@RequestBody User user) {
        log.info(user.getUsername());
        log.info(user.getPassword());

        QueryWrapper<User> wrapper = new QueryWrapper<>();

        wrapper.eq("username", user.getUsername());
        wrapper.eq("password",user.getPassword());
        // 数据库中查询用户信息
        User one = userService.getOne(wrapper);

        HashMap<String, String> result = new HashMap<>(); // 返回结果信息给前端

        if (one == null){
            result.put("code","401");
            result.put("msg","用户名或密码错误");
        }

        Map<String, String> map = new HashMap<>(); //用来存放payload信息

        map.put("id",one.getId().toString());
        map.put("username",one.getUsername());
        map.put("role",one.getRole());

        // 生成token令牌
        String token = JWTUtil.generateToken(map);

        // 返回前端token
        result.put("code","200");
        result.put("msg","登录成功");
        result.put("token",token);
        return result;
    }
}
```

### 8.启动Springboot

使用postman进行接口测试，查看是否返回了正确响应，并生成了token令牌

![image-20200811205734830](images/imagesimage-20200811205734830.png)



# 为Springboot配置拦截器

- 当用户登录成功时，服务端为该用户生成了一个时长为60的token令牌，`"token":xxx.xxx.xx`的形式 以返回给前端系统
- 当前端在每次请求时，我们选择将token令牌放至放入HTTP Header中的Authorization位
- 服务器对所有的请求进行拦截并校验token（除登录接口外）。如果token 信息不正确或过时，则向前端返回错误代码，校验正确则放行。

### 创建JWT拦截器

> 在此处进行token校验，如果校验成功则放行，否则返回异常信息给前端系统

```java
public class JWTInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        Map<String, String> map = new HashMap<>();

        //获取请求头中的token令牌
        String token = request.getHeader("token");
        try {
            JWTUtil.verify(token);//验证令牌
            return true;//放行请求
        } catch (SignatureVerificationException e) {
            e.printStackTrace();
            map.put("msg", "无效签名!");
        } catch (TokenExpiredException e) {
            e.printStackTrace();
            map.put("msg", "token过期!");
        } catch (AlgorithmMismatchException e) {
            e.printStackTrace();
            map.put("msg", "token算法不一致!");
        } catch (Exception e) {
            e.printStackTrace();
            map.put("msg", "token无效!!");
        }
        map.put("code", "403");//设置状态
        //将 map 转为json  jackson
        String json = new ObjectMapper().writeValueAsString(map);
        response.setContentType("application/json;charset=UTF-8");
        response.getWriter().println(json);
        return false;
    }
}
```

### 配置WebMvcConfigurer拦截器

> 在此处配置过滤规则：即添加JWTInterceptor token校验拦截器、拦截非登录接口外的一切/user接口

```java
@Configuration
public class MyWebMvcConfigurer implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new JWTInterceptor())
                .excludePathPatterns("/user/login") // 登录接口不用于token验证
                .addPathPatterns("/user/**"); // 其他非登录接口都需要进行token验证
    }
}
```



### 在控制器中添加测试接口

```java
@GetMapping(value = "/user/test")
public Map<String,String> test(HttpServletRequest request){
    
    Map<String, String> map = new HashMap<>();
    //处理自己业务逻辑
    String token = request.getHeader("token");
    DecodedJWT claims = JWTUtil.getTokenInfo(token);
    log.info("用户id: [{}]",claims.getClaim("id").asString());
    log.info("用户name: [{}]",claims.getClaim("username").asString());
    log.info("用户role: [{}]",claims.getClaim("role").asString());
    map.put("code","200");
    map.put("msg","请求成功!");
    return map;
}
```



### 启动Springboot进行测试

1. 访问http://localhost:8081/user/login进行登录测试，获取服务器端返回的token信息
2. 测试http://localhost:8081/user/test，在其请求头配置token信息

![image-20200811211951236](images/imagesimage-20200811211951236.png)