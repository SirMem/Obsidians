---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-09-14
Connected:
---
## 什么是AOP
- AOP:Aspect Oriented Programming(面向切面编程、面向方面编程)，其实就是面向特定方法编程。

- 实现：
- 动态代理是面向切面编程最主流的实现。而SpringAOP是Spring框架的高级技术，旨在管理bean对象的过程中，主要通过底层的动态代理机制，对特定的方法进行编程。

## 案例:统计各个业务层方法执行耗时


```xml
<!-- 在pom.xml中导入AOP的依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>

```

```java
// 编写AOP程序：针对于特定方法根据业务需要进行编程
@Component
@Aspect
public class TimeAspect {
	@Around("execution(* com.itheima.service.*,*(..))") //选定目录下的特定方法，切入点表达式，监听文件目录
    public Object recordTime(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        long begin = System.currentTimeMillis();

        Object object = proceedingJoinPoint.proceed();  // 调用原始方法运行

        long end = System.currentTimeMillis();
        Log.info(proceedingJoinPoint.getSignature() + "执行耗时: {}ms", end - begin);

        return object;
    }
}

```

## 场景
记录操作日志
权限控制
事务管理

## 优势
代码无侵入
减少重复代码
提高开发效率
维护便捷

## AOP核心概念
- 连接点：JoinPoint，可以被AOP控制的方法（暗含方法执行时的相关根据您提供的图片内容，以下是将其转为 Markdown 文本的描述，并标注了各个 AOP 概念在代码中的对应部分：

---

## AOP 核心概念

- **连接点 (JoinPoint)**：可以被 AOP 控制的方法（暗含方法执行时的相关信息）
- **通知 (Advice)**：指哪些重复的逻辑，也就是共性功能（最终体现为一个方法）
- **切入点 (PointCut)**：匹配连接点的条件，通知仅会在切入点方法执行时被应用
- **切面 (Aspect)**：描述通知与切入点的对应关系（通知 + 切入点）
- **目标对象 (Target)**：通知所应用的对象

---

### 切面类示例

```java
@Component
@Aspect
@Slf4j
public class TimeAspect {

    @Around("execution(* com.itheima.service.impl.DeptServiceImpl.list())")
    public Object recordTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long begin = System.currentTimeMillis();
        // 调用原始操作
        Object result = joinPoint.proceed();
        long end = System.currentTimeMillis();
        log.info("执行耗时：{} ms", (end - begin));
        return result;
    }
}
```

- **切面 (Aspect)**：`TimeAspect` 类本身即为切面类，定义了切入点和通知的逻辑。
- **通知 (Advice)**：`recordTime` 方法为通知，记录了方法的执行时间。
- **切入点 (PointCut)**：`@Around("execution(* com.itheima.service.impl.DeptServiceImpl.list())")`，定义了拦截的目标方法 `DeptServiceImpl.list()`。


### 目标对象示例

```java
@Service
public class DeptServiceImpl implements DeptService {

    @Autowired
    private DeptMapper deptMapper;

    @Override
    public List<Dept> list() {
        List<Dept> deptList = deptMapper.list();
        return deptList;
    }

    @Override
    public void delete(Integer id) {
        deptMapper.delete(id);
    }

    @Override
    public void save(Dept dept) {
        dept.setCreateTime(LocalDateTime.now());
        dept.setUpdateTime(LocalDateTime.now());
        deptMapper.save(dept);
    }
}
```

- **目标对象 (Target)**：`DeptServiceImpl` 类，即为通知应用的目标对象。
- **连接点 (JoinPoint)**：目标类中的每个方法（如 `list()`, `delete()`, `save()`）都是潜在的连接点，可以被 AOP 切入。
- **切入点 (PointCut)**：上面定义的 `list()` 方法是被切入的目标方法。
- 
## AOP执行流程
一旦进行了AOP程序的开发，那最终运行的就不再是原始的目标对象，而是基于目标对象自动所生成的代理对象

## AOP通知类型
1.@Around：环绕通知，此注解标注的通知方法在目标方法前、后都被执行
2．@Before：前置通知，此注解标注的通知方法在目标方法前被执行
3．@After：后置通知，此注解标注的通知方法在目标方法后被执行，无论是否有异常都会执行
4．@AfterReturning：返回后通知，此注解标注的通知方法在目标方法后被执行，有异常不会执行
5．@AfterThrowing：异常后通知，此注解标注的通知方法发生异常后执行

### 注意事项
@Around环绕通知需要自己调用ProceedingJoinPoint.proceed()来让原始方法执行，其他通知不需要考虑目标方法执行
@Around环绕通知方法的返回值，必须指定为object，来接收原始方法的返回值。

## @PointCut切入点
该注解的作用是将公共的切点表达式抽取出来，需要用到时引用该切点表达式即可。

```java
@Pointcut("execution(* ....)")
private void pt(){}

@Around("pt()")
public Object method(){}
```

## 通知顺序
当有多个切面的切入点都匹配到了目标方法，目标方法运行时，多个通知方法都会被执行。

执行顺序
1．不同切面类中，默认按照切面类的类名字母排序：
目标方法前的通知方法：字母排名靠前的先执行
目标方法后的通知方法：字母排名靠前的后执行
2．用@Order（数字）加在切面类上来控制顺序
目标方法前的通知方法：数字小的先执行
目标方法后的通知方法：数字小的后执行

## 切入点表达式
execution主要根据方法的返回值、包名、类名、方法名、方法参数等信息来匹配，语法为：
execution(访问修饰符? 返回值包名.类名.?方法名（方法参数）throws异常?）
- 其中带?的表示可以省略的部分
- 访问修饰符：可省略(比如：public、protected)
- 包名，类名：可省略
- throws异常：可省路(注意是方法上声明抛出的异常，不是实际抛出的异常)

通配符号

`*` : 单个独立的任意符号，可以通配任意返回值、包名、类名、方法名、任意类型的一个参数，也可以通配包、类、方法名的一部分
```java
execution(* com.*.service.*.update*(*)) //update开头，后面是什么无所谓
```

`..`: 多个连续的任意符号，可以通配任意层级的包，或任意类型、任意个数的参数
```java
execution(* com.new-boy..DeptService.*(..))
```

```java
@Before("execution(public void com.new-boy.service.impl.DeptServiceImpl.delete(java.lang.Integer))")

public void before(JoinPoint joinPoint)
```
可以使用且(&&)、或（||）、非(!)来组合比较复杂的切入点表达式。

书写规范:

- 所有业务方法名在命名时尽量规范，方便切入点表达式快速匹配。如：查询类方法都是find开头，更新类方法都是update开头。

- 描述切入点方法通常基于接口描述，而不是直接描述实现类，增强拓展性。
- 在满足业务需要的前提下，尽量缩小切入点的匹配范围。如：包名匹配尽量不使用，使用`*` 匹配单个包。


@annotation切入点表达式，用于匹配标识有特定注解的方法。

## 连接点
- 在Spring中用JoinPoint抽象了连接点，用它可以获得方法执行时的相关信息，如目标类名、方法名、方法参数等。
- 对于@Around通知，获取连接点信息只能使用ProceedingJoinPoint
- 对于其他四种通知，获取连接点信息只能使用JoinPoint,它是ProceedingJoinPoint的父类型