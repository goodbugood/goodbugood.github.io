# Spring常见的3种自动注入方式


## 注解 @Autowired/@Resource 注入

- `@Autowired` 默认按类型查找，当一个类型多个实现时，按名字找。按名字查找时，配合 `@Qualifier` 注解。
- `@Resource` 默认按名字找，找不到时，按类型找。

```java
public class IndexController {
    @Autowired
    private UserService userService;
}
```

这种注入方式不容易理解，属性的可访问性是私有的，即无 set 方法，也无有参构造，那是如何从外部注入的呢？

**Spring 再怎么玩的花，也不能脱离 Java 的语法限制。它在注入前，修改了私有属性的可访问性，然后再注入。**

```java
userService.setAccessible(true);
```

## setXXX方法注入

```java
public class IndexController {
    private UserService userService;
    
    @Autowired
    public void setUserService(UserService userService) {
        this.userService = userService;
    }
}
```

## 有参构造方法注入

```java
```


