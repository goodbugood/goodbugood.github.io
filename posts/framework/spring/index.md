# Spring


在旧的开发模式下，一个组件如果要使用另一个组件，必须先知道如何正确地创建它。于是引入IoC。

> IoC又称为依赖注入（DI：Dependency Injection），它解决了一个最主要的问题：将组件的创建+配置与组件的使用相分离，并且，由IoC容器负责管理组件的生命周期。

## 常用注解

| 注解           | 功能描述                                                     | 搭档注解                   |
| -------------- | ------------------------------------------------------------ | -------------------------- |
| @Component     | 通常用来注解类，声明此类是一个 Spring Bean，Spring 启动后要交给 Spring 创建和托管的类。<br />==JSR-330 标准推荐使用 @Named== | @Autowired                 |
| @Autowired     | 让 Spring 按照变量类型，从 Spring 容器中获取对应的实例进行绑定。<br />即按类型注入，如果`UserServiceI`接口存在多个实现类时，就不知道注入哪个了，报错。<br />==JSR-330 标准推荐使用 @Inject== | @Qualifier<br />@Component |
| @Qualifier     | 按具体的实现类类名注入，@Qualifier("UserServiceImpl")<br />==JSR-330 标准推荐使用 @Named== |                            |
| @Resource      | 同 @Autowired，@Qualifier 功能相同，都是属性注入。<br />不同点就是他们的注入依据不同，@Resource(name="UserServiceImpl") 按类名<br />Java 内置注解 |                            |
| @Controller    | 声明此类是 Web 层的一个控制器                                |                            |
| @Service       | 注解这是 Service 层的一个类                                  |                            |
| @Repository    | 注解这是 Dao 层的一个类                                      |                            |
| @PostConstruct | 注解方法，说这是一个执行类构造方法的时候执行的方法，就是一个钩子方法，或者说标签。<br />类似 PHPUnit 中的`setUp()`方法 | @PreDestory                |
| @PreDestory    | 注解方法，说这是一个执行类的析构方法`finalize()`时执行的方法，也是一个钩子方法。<br />类似 PHPUnit 中的`tearDown()`方法 |                            |

@Component，@Controller，@Service，@Repository，这几个注解的作用都是生产 bean，都是注解在类上，将类注解成 spring 的 bean 工厂中一个一个的 bean。

其实 @Controller，@Service，@Repository 就是语义更加细化的 @Component。不仅能说明此类是 bean，还能说明此 bean 用于哪一层。

## 搭配使用的注解

### @Bean 必须搭配 @Configuration 吗

1. @Bean 注解在方法上，想要使用必须扫到此文件，所以类文件上必须加注解 @Component
2. @Configuration 也实现了 @Component
3. 之所有用 @Configuration，而不使用其他的 @Component，是因为他的语义化更强

`@Import` 与 `@Bean` 注解功能类似，注入第三方功能。

### 通过名称获取 Spring Bean 时，@Autowired 必须搭配 @Qualifier

## Spring Bean 的生命周期

![image-20230409173105558](./images/image-20230409173105558.png)

## 易错点

### @Transactional注解事务使用

- 使用`@Transactional`注解事务时，务必指明什么异常回滚，否则`Spring`只会遇到`RuntimeException`和`Error`异常时才回滚。`@Transactional(rollbackFor = Exception.class)`。
- `@Transactional`注解修饰的方法里，不要捕获异常，因为你捕获了异常，他也就无法感知异常，故无法回滚。推荐业务层方法用`@Transactional`修饰，统一抛出异常，控制器层统一处理异常。

