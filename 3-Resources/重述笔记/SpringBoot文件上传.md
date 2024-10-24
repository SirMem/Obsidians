---
Project: 
Status:
tags: Resources
Deadline:
CreateTime:
Connected:
---

## 简介
- 文件上传，是指将本地图片、视频、音频等文件上传到服务器，供其他用户浏览或下载的过程。
- 文件上传在项目中应用非常广泛，我们经常发微博、发微信朋友圈都用到了文件上传功能。

## 前端三要素
Here is the HTML code based on the image provided:

```html
<form action="/upload" method="post" enctype="multipart/form-data">
    姓名: <input type="text" name="username"><br>
    年龄: <input type="text" name="age"><br>
    头像: <input type="file" name="image"><br>
    <input type="submit" value="提交">
</form>
```

file
method: post
enctype= "multipart/form-data"

## 服务端响应文件
Here is the code from the image you uploaded:

```java
@RestController
public class UploadController {

    @PostMapping("/upload")
    public Result upload(String username, Integer age, MultipartFile image) {
        return Result.success();
    }

}
```


MultipartFile
- String getOriginalFilename();/获取原始文件名
- void transferTo(File dest);/将接收的文件转存到磁盘文件中
- long getSize();/获取文件的大小，单位：字节
- byte `[]` getBytes();/获取文件内容的字节数组
- InputStream getInputStream();/获取接收到的文件内容的输入流

文件大小
```java
spring.servlet.multipart.max-file-size=10MB  //每个文件请求
spring.servlet.multipart.max-request-size=10MB //单次请求的所有文件
```

## 第三方服务
SDK : SDK:Software Development Kit的缩写，软件开发工具包，包括辅助软件开发的依颗Jar包）、代码示例等，都可以叫做SDK

Bucket:存储空间是用户用于存储对象(Object,就是文件)的容器，所有的对象都必须隶属于某个存储空间。


## 配置文件
@value注解，将appication.properties对应的key注入到相应的变量中

具体用法: @Value("$(配置文件中的key)")
```properties
aliyun.oss.endpoint = https://oss-cn-hangzhou.aliyuns.com
```

```java
@Value("$(aliyun.oss.endpoint)")
private String endpoint;
```

### yml(yaml)配置
层级配置
key : value

```yml
server:
	port: 8080
```

推荐使用yml作为配置文件

### 基本语法
- 大小写敏感
- 数值前边必须有空格，作为分隔符
- 使用缩进表示层级关系，缩进时，不允许使用Tab键，只能用空格(idea中会自动将Tab转换为空格)
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- `#` 表示注释，从这个字符一直到行尾，都会被解析器忽略

### @ConfigurationProperties
将配置文件相关的key和value注入到变量中

使用方法
- 在类上声明@Component
- 必须给变量声明get和set方法，可以使用@Data
- 在类上声明ConfigurationProperties，语法如下@ConfigurationProperties(prefix = "配置文件key的前缀")

如
```yml
aliyun:
	oss:
		endpoint: https://oss-cn-hangzhou.aliyuns.com
```

```java
@Data
@Component
@ConfigurationProperties(prefix = "aliyun.oss")
public class AliOSSProperties{
	private String endpoint;
}
```

在其他实体类中使用通过ConfigurationProperties注解的变量，可以通过调用Bean对象的Autowired注解，并使用创建对象的方法来获取变量值

```java
@Autowired
private AliOSSProperties aliOSSProperties;

String endpoint = aliOSSProperties.getEndpoint(); //获取配置文件value
```

spring boot configuration processor : 提示相关配置项，提示被@ConfigurationProperties注入的Bean对象，提示其对象的属性名相对应的配置项的名字