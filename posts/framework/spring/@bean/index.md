# Bean搭配Configuration和Component


# @Bean搭配@Configuration和@Component

背景：

```java
@org.springframework.stereotype.Component
public @interface Configuration {
}
```

我们都知道 `@Bean` 需要搭配 `@Configuration` 注解才能使得定义的配置类被扫描到。

但是我们发现 `@Configuration` 注解也被 `@Component` 修饰，那么就能替换，达到被扫描的效果，还真是。

## 不同

1. @Bean 搭配 @Component 使用，被 @Bean 修饰的方法不能彼此调用
2. 局限性，@Configuration 注解的类不能被 final 修饰，因为 Spring 要使用 CGLIB 子类化注解的类，但 final 类无子类


