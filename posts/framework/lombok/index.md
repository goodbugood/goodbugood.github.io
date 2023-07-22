# Lombok


## 安装

### Maven

```xm
<dependencies>
	<dependency>
		<groupId>org.projectlombok</groupId>
		<artifactId>lombok</artifactId>
		<version>1.18.24</version>
		<scope>provided</scope>
	</dependency>
</dependencies>
```

| 官方文档                              | 描述       |
| ------------------------------------- | ---------- |
| https://projectlombok.org/changelog   | 变更日志   |
| https://projectlombok.org/setup/maven | 安装手册   |
| https://projectlombok.org/features/   | 使用说明书 |

下面是一些常用的注解。

## var

`val` 等效于使用 `final var`。

```java
val str = "hello";
final String str2 = str;
// 上面 2 种方式是等效的
Assert.assertSame(str2, str);
```

用来修饰局部变量，相当于将变量声明为 final。

## @NonNull

修饰方法形参，如果形参为 null，则抛`NullPointerException`。

## @Cleanup

@Cleanup 相当于 finally 模块，主要用来释放资源的。

## @Setter

帮你实现一些`setXXX()`方法

## @Getter

帮你实现一些`getXXX()`方法

==注意==：

如果属性是基本类型`boolean`，则 get 方法的前缀不是 get 而是 `isXXX`。如果使用 boolean 的包装类型`Boolean`，则方法前缀依旧保持为 get。

## @ToString

见名知意，就是帮你实现`toString()`方法。

### 排除某个字段属性被 toString

```java
import lombok.Data;

@Data
public class Tony {
    @ToString.Exclude
    private Integer salary;
}
```

被`@ToString.Exclude`注解修饰的属性不会被添加到 toString 中。

## @EqualsAndHashCode

## @NoArgsConstructor

## @RequiredArgsConstructor

为类属性生成构造方法。主要为以下属性生成构造方法。

1. 被 `@NonNull` 注解的属性
2. 被 `final` 修饰的常量属性

### 用处

#### 配合 final 生成构造方法注入

```java
import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor
public class IndexController {
    private final UserService userService;
}
```

我们看字节码文件。

```java
public class IndexController {
    private final UserService userService;
    
    public IndexController(UserService userService) { /* compiled code */ }
}
```

从而实现了构造方法注入依赖。

## @AllArgsConstructor

## @Data

> All together now: A shortcut for @ToString, @EqualsAndHashCode, @Getter on all fields, and @Setter on all non-final fields, and @RequiredArgsConstructor!

大概意思就是说这个注解，1 个顶 5 个。

具体顶哪 5 个，就是上面描述的。

源码：

```java
@Data
class UserInfo {
    private String address;
}
```

编译完的

字节码 class 文件：

```java
class UserInfo {
    private java.lang.String address;

    public UserInfo() { /* compiled code */ }

    public java.lang.String getAddress() { /* compiled code */ }

    public void setAddress(java.lang.String address) { /* compiled code */ }

    public boolean equals(java.lang.Object o) { /* compiled code */ }

    protected boolean canEqual(java.lang.Object other) { /* compiled code */ }

    public int hashCode() { /* compiled code */ }

    public java.lang.String toString() { /* compiled code */ }
}
```

从字节码中看，补充了`getXXX`，`setXXX`，`equals`，`canEqual`，`hashCode`，`toString`。

### 排除某个方法

@Data 默认帮我们实现了多个方法，如果我们不想实现其中的某个方法，如何排除掉呢，不支持。

## @Value

相当于 @Data，且给属性加上 final ，但是没有`setXXX`方法。
