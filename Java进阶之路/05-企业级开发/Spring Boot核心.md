---
created: 2026-06-08
source: https://javabetter.cn/springboot/
tags: [java, spring, springboot]
优先级: ⭐⭐⭐
---

# Spring Boot 核心

> 来源：[二哥的Java进阶之路](https://javabetter.cn/springboot/)

---

## 一、Spring Boot 核心特性

| 特性 | 说明 |
|------|------|
| 自动配置 | `@EnableAutoConfiguration` + spring.factories |
| 起步依赖 | 整合常用依赖，如 `spring-boot-starter-web` |
| 嵌入式容器 | Tomcat/Jetty/Undertow 内嵌 |
| 生产就绪 | 健康检查、指标、外部化配置 |

### 核心注解
```java
@SpringBootApplication = @Configuration + @EnableAutoConfiguration + @ComponentScan
```

---

## 二、IoC（控制反转）与 DI（依赖注入）

**核心思想**：对象的创建和管理交给 Spring 容器

```java
// 组件标记
@Component / @Service / @Repository / @Controller / @RestController

// 依赖注入
@Autowired      // 按类型注入
@Resource       // 按名称注入
@Inject         // JSR-330 规范

// 配置类
@Configuration
public class AppConfig {
    @Bean
    public DataSource dataSource() { return new HikariDataSource(); }
}
```

**Bean 作用域**：
| 作用域 | 说明 |
|--------|------|
| singleton | 单例（默认） |
| prototype | 每次创建新实例 |
| request | 一次 HTTP 请求 |
| session | 一个 HTTP Session |

**Bean 生命周期**：
```
实例化 → 属性赋值 → BeanNameAware → BeanFactoryAware
→ BeanPostProcessor#postProcessBeforeInitialization
→ @PostConstruct / InitializingBean
→ BeanPostProcessor#postProcessAfterInitialization
→ 使用中 → @PreDestroy / DisposableBean
```

---

## 三、AOP（面向切面编程）

### 核心概念
| 概念 | 说明 |
|------|------|
| Join Point | 方法执行点 |
| Pointcut | 切面表达式，如 `execution(* com..*.*(..))` |
| Advice | 增强逻辑：@Before/@After/@Around/@AfterReturning/@AfterThrowing |
| Aspect | 切面 = Pointcut + Advice |

```java
@Aspect
@Component
public class LogAspect {
    @Around("execution(* com.example.service.*.*(..))")
    public Object logAround(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = pjp.proceed();
        long elapsed = System.currentTimeMillis() - start;
        System.out.println(pjp.getSignature() + " 执行耗时: " + elapsed + "ms");
        return result;
    }
}
```

**AOP 底层**：
- 有接口 → JDK 动态代理（基于接口）
- 无接口 → CGLIB 动态代理（基于继承）

---

## 四、Spring MVC 工作流程

```
请求 → DispatcherServlet → HandlerMapping → HandlerAdapter
      ← ModelAndView ← 执行 Controller
      → ViewResolver → 渲染 View → 响应
```

---

## 五、事务管理 @Transactional

```java
@Service
public class UserService {
    @Transactional(rollbackFor = Exception.class)
    public void createUser(User user) {
        userDao.insert(user);
        // 抛出异常时事务回滚
    }
}
```

**事务传播行为（Propagation）**：
| 类型 | 说明 |
|------|------|
| REQUIRED（默认） | 支持当前事务，没有则新建 |
| REQUIRES_NEW | 新建事务，挂起当前事务 |
| SUPPORTS | 支持当前事务，没有就不使用 |
| NOT_SUPPORTED | 以非事务方式执行 |
| MANDATORY | 必须已有事务，否则抛异常 |
| NEVER | 不能有事务 |
| NESTED | 嵌套事务（JDBC Savepoint） |

**注意事项**：
- 默认只回滚 `RuntimeException` 和 `Error`
- 同类方法中直接调用 `@Transactional` 失效（代理机制）
- 可以通过 `rollbackFor` 指定回滚异常

---

## 六、Spring Boot 与 MyBatis

```java
// 1. Mapper 接口
@Mapper
public interface UserMapper {
    @Select("SELECT * FROM user WHERE id = #{id}")
    User findById(Long id);
}

// 2. XML 方式（resource/mapper/UserMapper.xml）
<mapper namespace="com.example.mapper.UserMapper">
    <select id="findById" resultType="User">
        SELECT * FROM user WHERE id = #{id}
    </select>
</mapper>

// 3. 配置文件
mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.example.entity
```

---

## 💡 面试高频题

1. **Spring 的 IoC 和 DI 区别？** → IoC 是思想，DI 是实现方式
2. **Bean 的线程安全？** → 单例非线程安全，无状态/ThreadLocal 解决
3. **@Autowired 和 @Resource 区别？** → 按类型 vs 按名称
4. **循环依赖怎么解决？** → 三级缓存（singletonFactories）
5. **事务失效场景？** → 同类方法调用、非 public 方法、自定抛异常类型
6. **MyBatis 一级/二级缓存？** → 一级 SqlSession，二级 Mapper 级别
