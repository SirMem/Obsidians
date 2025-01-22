---
Project: 
Status:
tags: Resources
Deadline:
CreateTime: 2024-11-13
Connected:
---

#review

![](https://i.sstatic.net/9iCub.png)
## Filter
## HttpFilter
`HttpFilter` 是 `javax.servlet.Filter` 接口的一个子类，专门用于 HTTP 请求的过滤。它是 Servlet 3.0 引入的，用于简化处理 HTTP 请求和响应的过滤器。通过继承 `HttpFilter`，你可以直接实现过滤器而不需要显式实现 `Filter` 接口，并且它会自动支持 `doFilter` 方法。

#### 关键方法：

- **`doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain)`**：
    - 这是最重要的方法，`HttpFilter` 会在 HTTP 请求和响应流通过时进行处理。在这里，你可以修改请求、响应，或者进行一些预处理操作（如日志记录、认证等）。
    - `chain.doFilter(request, response)`：执行该方法会将请求传递给下一个 Filter 或最终的 Servlet。

```java
public class MyFilter extends HttpFilter {
    @Override
    protected void doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        // 在请求到达 Servlet 之前的预处理
        response.addHeader("My-Custom-Header", "SomeValue");

        // 传递给下一个 Filter 或 Servlet
        chain.doFilter(request, response);

        // 响应发送之前的后处理
    }
}

```

## HttpServletRequest
`HttpServletRequest` 是一个接口，表示客户端发送到服务器的 HTTP 请求。它提供了方法用于访问 HTTP 请求的各种信息，包括请求的 URL、请求参数、请求头、Cookie 等。

#### 常用方法：

- **`getMethod()`**：获取 HTTP 请求方法（如 GET、POST、PUT、DELETE 等）。
- **`getRequestURI()`**：获取请求的 URI（统一资源标识符）。
- **`getHeader(String name)`**：获取指定名称的请求头。例如，获取 `Origin` 请求头用于处理 CORS。
- **`getParameter(String name)`**：获取请求中的参数。
- **`getCookies()`**：获取请求中的所有 Cookie。

#### 作用：

- `HttpServletRequest` 主要用于获取客户端请求的相关信息，比如请求的路径、请求头、请求参数等。

```java
String userAgent = request.getHeader("User-Agent");
String requestMethod = request.getMethod();
```

## HttpServletResponse
`HttpServletResponse` 是一个接口，表示服务器发送到客户端的 HTTP 响应。它提供了方法来修改响应头、设置响应状态码、写入响应体等。
#### 常用方法：

- **`setStatus(int statusCode)`**：设置响应的状态码（如 200、404 等）。
- **`setHeader(String name, String value)`**：设置响应头。
- **`addHeader(String name, String value)`**：添加响应头（不会覆盖现有的头部）。
- **`getWriter()`**：获取可以将响应内容写入的 `PrintWriter` 对象。
- **`getOutputStream()`**：获取响应内容的输出流。

#### 作用：

- `HttpServletResponse` 主要用于设置响应的相关信息，如响应状态码、响应头和响应体。

```java
response.setStatus(HttpServletResponse.SC_OK);
response.addHeader("Access-Control-Allow-Origin", "*");
response.getWriter().write("Hello, world!");

```


## FilterChain
`FilterChain` 是 `doFilter` 方法中的一个参数，表示一个 Filter 链。它控制请求和响应如何流动，从一个 Filter 到下一个 Filter，直到最终的 Servlet 处理。

#### 常用方法：

- **`doFilter(ServletRequest request, ServletResponse response)`**：将请求和响应传递给 Filter 链中的下一个过滤器或者最终的 Servlet。

#### 作用：

- `FilterChain` 用于在多个 Filter 之间传递请求和响应。如果你有多个 Filter，它们会按照配置的顺序依次执行。

```java
@Override
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
        throws IOException, ServletException {
    // 在这里执行请求的预处理操作
    chain.doFilter(request, response); // 将请求传递给下一个 Filter 或 Servlet
    // 在这里执行响应的后处理操作
}

```

## ServletException
`ServletException` 是一个异常类，表示 Servlet 在处理请求时发生的异常。它继承自 `Exception` 类，通常用于表示由于请求处理问题导致的错误。

#### 常用用途：

- 如果在 `Filter` 或 `Servlet` 中发生了错误，通常会抛出 `ServletException`。
- 它也可以携带其他类型的异常（作为原因）通过构造函数传递。

#### 作用：

- `ServletException` 用于标识 Servlet 相关的错误，特别是在请求处理过程中发生的异常。


```java
try {
    // 处理请求
    chain.doFilter(request, response);
} catch (Exception e) {
    throw new ServletException("Error occurred during request processing", e);
}

```