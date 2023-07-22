# 三问Java


其实也不止三问。

## java.io.Serializable

为什么实现 `interface java.io.Serializable` 的类中都定义一个非大写的类常量。

```java
private static final long serialVersionUID = 1L;
```

## SLF4J的日志工厂

为什么`org.slf4j.LoggerFactory`工厂要为每一个类都实例化一个 logger 实例。

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

private final static Logger logger = LoggerFactory.getLogger(getClass());
```

### 实例属性，类属性，Spring Bean 的 logger 区别

```java
// 一：类属性 logger
@Service
class UserServiceImpl {
    private static final Logger log = LoggerFactory.getLogger(UserServiceImpl.class);
}

// 二：实例属性 logger
@Service
class UserServiceImpl {
    private final Logger log = LoggerFactory.getLogger(UserServiceImpl.class);
}

// 三：spring bean logger
@Service
class UserServiceImpl {
    @Autowired
    private Logger log;
}

@Configuration
class MyConfig {
    @Bean
    public Logger logger() {
        return LoggerFactory.getLogger("mylogger");
    }
}
```

区别：

1. 方式一是单例，懒汉模式
2. 方式二多例
3. 方式二和方式一能够记录调用者所在类信息，文件，行号

