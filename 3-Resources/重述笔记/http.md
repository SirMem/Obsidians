---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 
Connected:
---

## HTTP
- 概念：Hyper Text Transfer Protocol,超文本传输协议，规定了浏览器和服务器之间数据传输的规则。
- 特点：
	1. 基于TCP协议：面向连接，安全
	2. 基于请求-响应模型的：一次请求对应一次响应
	3. HTTP协议是无状态的协议：对于事务处理没有记忆能力。每次请求-响应都是独立的。
- 缺点：多次请求间不能共享数据。
- 优点：速度快

## 请求协议
```http
GET /brand/findAll?name=OPPO&status=1 HTTP/1.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/...

# 请求行: GET 请求，包含请求方式、资源路径和 HTTP 协议版本
# 请求头: 指定接收的数据格式、压缩方式、语言等客户端的信息

POST /brand HTTP/1.1
Accept: application/json, text/plain, */*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Content-Length: 161
Content-Type: application/json;charset=UTF-8
Cookie: Idea-8296eb32=841b16f0-0cfe-495a-9cc9-d5aaa71501a6; JSESSIONID=0FDE4E430876BD9C5C955F061207386F
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/...

{
    "status": 1,
    "brandName": "黑马",
    "companyName": "黑马程序员",
    "id": "",
    "description": "黑马程序员"
}

# 请求行: POST 请求，指定资源路径 /brand 和 HTTP 协议版本
# 请求头: 说明客户端支持的内容类型、编码格式、语言等信息
# 请求体: 包含 JSON 格式的数据，提交到服务器


```

## 响应协议
```http

HTTP/1.1 200 OK
Content-Type: application/json
Transfer-Encoding: chunked
Date: Tue, 10 May 2022 07:51:07 GMT
Keep-Alive: timeout=60
Connection: keep-alive

[
    {
        "id": 1,
        "brandName": "阿里巴巴",
        "companyName": "腾讯计算机系统有限公司",
        "description": "玩玩玩"
    }
]

# 响应行: HTTP 协议版本、状态码（200 OK）
# 响应头: 响应的元数据信息，包括内容类型、传输编码、连接状态等
# 响应体: 实际的返回数据，JSON 格式，包含品牌的相关信息
```

这个代码将图片中的 HTTP 响应信息完整地转化为代码，包含响应行、响应头和响应体的内容。

### HTTP 状态码

|状态码|含义|
|---|---|
|1xx|响应中 - 临时状态码，表示请求已经接收，告诉客户端应该继续请求或者忽略它如果它已经完成。|
|2xx|成功 - 表示请求已经成功接收，处理已完成。|
|3xx|重定向 - 重定向到其他地方；让客户端再发起一次请求以完成整个处理。|
|4xx|客户端错误 - 处理发生错误，责任在客户端。如：请求了不存在的资源、客户端未被授权、禁止访问等。|
|5xx|服务器错误 - 处理发生错误，责任在服务端。如：程序抛出异常等。|

### HTTP 响应头

|响应头|说明|
|---|---|
|Content-Type|表示该响应内容的类型，例如 `text/html`，`application/json`。|
|Content-Length|表示该响应内容的长度（字节数）。|
|Content-Encoding|表示响应压缩算法，例如 `gzip`。|
|Cache-Control|指示客户端应如何缓存，例如 `max-age=300` 表示可以最多缓存 300 秒。|
|Set-Cookie|告诉浏览器为当前所在的域设置 `cookie`。|

## Web服务器
- Web服务器是一个软件程序，对HTTP协议的操作进行封装，使得程序员不必直接对协议进行操作，让Web开发更加便捷
- 主要功能是"提供网上信息浏览服务"。