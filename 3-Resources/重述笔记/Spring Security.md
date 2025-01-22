---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-11-15
Connected:
---

#review

## 自定义校验
自行实现UserDetailsService或是功能更完善的UserDetailsManager接口
```java
@Service  
public class AuthorizeService implements UserDetailsService {  
  
    @Resource  
    UserMapper mapper;  
  
    @Override  
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {  
        Account account = mapper.findUserByName(username);  
        if(account == null)  
            throw new UsernameNotFoundException("用户名或密码错误");  
        return User  
                .withUsername(username)  
                .password(account.getPassword())  
                .build();  
    }  
}
```

## 全局授权

```java
.authorizeHttpRequests(auth -> {  
        //静态资源依然全部可以访问  
        auth.requestMatchers("/static/**").permitAll();  
//只有具有以下角色的用户才能访问路径"/"  
    auth.requestMatchers("/").hasAnyRole("user", "admin");  
//其他所有路径必须角色为admin才能访问  
    auth.anyRequest().hasRole("admin");  
})
```


## 注解授权
可以以注解形式直接配置，首先需要在配置类（注意这里是在Mvc的配置类上添加，因为这里只针对Controller进行过滤，所有的Controller是由Mvc配置类进行注册的，如果需要为Service或其他Bean也启用权限判断，则需要在Security的配置类上添加）上开启
```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity   //开启方法安全校验

public class SecurityConfiguration {
    ...
}
```

在想要进行权限校验的接口添加注解
```java
@Controller
public class HelloController {
    @PreAuthorize("hasRole('user')")  //直接使用hasRole方法判断是否包含某个角色
    @GetMapping("/")
    public String index(){
        return "index";
    }

    ...
}
```

通过添加`@PreAuthorize`注解，在执行之前判断判断权限，如果没有对应的权限或是对应的角色，将无法访问页面。

除了Controller以外，只要是由Spring管理的Bean都可以使用注解形式来控制权限，我们可以在任意方法上添加这个注解，只要不具备表达式中指定的访问权限，那么就无法执行方法并且会返回403页面。

`@PostAuthorize`注解，是在方法执行之后再进行拦截

## 参数过滤
可以使用`@PreFilter`和`@PostFilter`对集合类型的参数或返回值进行过滤。
```java
@PreFilter("filterObject.equals('lbwnb')")   //filterObject代表集合中每个元素，只要满足条件的元素才会留下
public void test(List<String> list){
    System.out.println("成功执行"+list);
}
```