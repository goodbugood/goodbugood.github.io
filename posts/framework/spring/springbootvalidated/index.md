# SpringBoot 参数校验


## 验证注解

### @NotNull，@NotEmpty，@NotBlank 三者的区别

注意：

1. @NotBlank 只能用来注解`String`
2. @NotEmpty 只能注解`字符串`和`集合`和`数组`

|                     | @NotNull | @NotEmpty | @NotBlank |
| ------------------- | -------- | --------- | --------- |
| String name = null; | false    | false     | false     |
| String name = "";   | true     | false     | false     |
| String name = " ";  | true     | true      | false     |
| String name = "a";  | true     | true      | true      |

总结：

1. `@NotNull`要求字段必传，且不能为 null
2. `@NotEmpty`要求字段必传，且不能为 null
3. `@NotBlank`仅可修饰字符串，除了要求必传非 null，且要求字符串除了不可见字符，必须是有可见字符的

## 普通验证

## 分组验证

## 常见坑

### 参数上的校验注解只能校验 JavaBean

```java
@Data
public class User {
    @NotNull(message = "用户id不能为空")
    private Integer id;
}
```

1. 仅使用`@Valid`和`@Validated`只能校验 JavaBean，无法校验`List<JavaBean>`

```java
public class UserController {
    public String addUser(@Valid @RequestBody List<User> userList)
}
```

```java
public class UserController {
    public String addUser(@Validated @RequestBody List<User> userList)
}
```

上述校验方法根本不起作用，也就是校验不生效。

2. 控制器类加`@Validated`注解，控制器方法形参加`@Validated`也无法完成验证

```java
@Validated
public class UserController {
    public String addUser(@Validated @RequestBody List<User> userList)
}
```

**那这里就有疑问了，不能使用`@Validated`，分组验证也不能用了吗？**

3. 控制器类使用`@Validated`注解，控制器方法形参使用`@Valid`注解，方可验证集合

```java
@Validated
public class UserController {
    public String addUser(@Valid @RequestBody List<User> userList)
}
```

## 参考

- [Validated数据校验，看这一篇就够了](https://blog.csdn.net/weixin_43990804/article/details/112974137)

- [使用@Validated校验List接口参数的两种方式](https://blog.csdn.net/cherrish_1991/article/details/119342061)
