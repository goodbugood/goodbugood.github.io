# Java之为什么呢


## 使用常量类还是常量接口，为什么呢

比如人类，有性别男女，你认为我们是定义到人类类里，还是性别接口里呢？孰优孰劣？

```java
final class People {
    // 性别 男
    public static final String GENDER_MAN = "男";
    // 性别 女
    public static final String GENDER_WOMAN = "女";

    private People() {
    }
}

interface Gender {
    // 性别 男
    String GENDER_MAN = "男";
    // 性别 女
    String GENDER_WOMAN = "女";
}
```

### 接口

1. 接口中的成员常量均是`public final`类型的，且可省略
2. 接口中只有常量，没有变量
3. 接口中的常量会被实现类继承，导致继承的常量可能与子类中的常量冲突
4. 由于接口中的常量都是`public`，导致有些常量可能是仅内部需要，也被暴露出去了
5. 而且常量接口中的常量，还会污染其实现类未来的子类，所以为了避免常量类中出现这种情况，我们通常用`final`来定义常量类。

### 类

1. 类可以通过`final`修饰，使其无法被继承，故类常量不会对子类常量造成冲突
2. 私有构造方法，防止被实例化

### 反例

Java Swing 框架中的 [SwingConstants](https://docs.oracle.com/javase/6/docs/api/javax/swing/SwingConstants.html) 接口中就定义了大量静态常量。

## 常量应该存在哪里？枚举类？常量类？常量接口中？

### 大家说

1. 接口是抽象，可能常量接口里定义了很多常量，而实现者可能仅需要几个常量，却得到了更多无用的常量。

反驳：为何不把接口的粒度定小点，常量也应该分业务来管理。

### 参考

- 《Effective Java》作者原话

> According to Joshua Bloch, author of "Effective Java": The constant interface pattern is a poor use of interfaces. That a class uses some constants internally is an implementation detail. Implementing a constant interface causes this implementation detail to leak into the class's exported API.  It is of no consequence to the users of a class that the class implements a constant interface.  In fact, it may even confuse them. Worse, it represents a commitment:  if in a future release the class is modified so that it no longer needs to use the constants, it still must implement the interface to ensure binary compatibility. If a nonfinal class implements a constant interface, all of its subclasses will have their namespaces polluted by the constants in the interface.

- [Interfaces with static fields in java for sharing 'constants'](https://stackoverflow.com/questions/320588/interfaces-with-static-fields-in-java-for-sharing-constants)

下面看看，AI 是如何回答的：

```
作为AI语言模型，我无法给出具体建议，因为这取决于具体的应用场景和需求。但是，以下是常量类和常量接口的一些优缺点，供您参考：

常量类的优点：

可以包含非常量的成员变量和方法；
可以使用继承和实现接口的方式来扩展常量；
可以使用静态导入来方便地使用常量。
常量类的缺点：

常量类的成员变量必须是final修饰的，因此无法修改；
常量类的成员变量必须在编译时就确定，因此无法动态添加常量；
常量类的成员变量必须在类加载时就初始化，因此可能会影响性能。
常量接口的优点：

可以方便地定义大量常量；
可以使用继承和实现接口的方式来扩展常量；
可以使用静态导入来方便地使用常量。
常量接口的缺点：

常量接口的成员变量必须是public static final修饰的，因此无法修改；
常量接口的成员变量必须在编译时就确定，因此无法动态添加常量；
常量接口的成员变量必须在类加载时就初始化，因此可能会影响性能；
常量接口的成员变量可能会被其他类误用或者修改，因此不够安全。
综上所述，常量类和常量接口都有各自的优缺点，具体使用哪个取决于具体的需求和场景。
```

## 类常量为什么还有小写

我们看到一些优秀的代码，或者是编码规约，框架代码，都在强调类常量名称要大写，但是有些却不是。

### 没有执行类常量名称大写的情况

```java
public final class ch.qos.logback.classic.Logger implements org.slf4j.Logger, LocationAwareLogger, AppenderAttachable<ILoggingEvent>, Serializable {
	// 小驼峰类常量
    private static final long serialVersionUID = 5454405123156820674L; // 8745934908040027998L;

    // 大写类常量
    public static final String FQCN = ch.qos.logback.classic.Logger.class.getName();
}
```

lombok 框架的 `@Slf4j` 注解：

```java
// 小驼峰类常量
private static final org.slf4j.Logger log;
```

### 总结

细心的你会发现如下结论：

1. `public` 类常量，常量名称全大写，多个单词下划线分割
2. 非 `public` 类常量，常量名称小驼峰

哈哈，其实上面的总结也是错误的。不信你看 `java.time.Year`

```java
public final class Year
        implements Temporal, TemporalAdjuster, Comparable<Year>, Serializable {

    /**
     * The minimum supported year, '-999,999,999'.
     */
    public static final int MIN_VALUE = -999_999_999;
    /**
     * The maximum supported year, '+999,999,999'.
     */
    public static final int MAX_VALUE = 999_999_999;

    /**
     * Serialization version.
     */
    private static final long serialVersionUID = -23038383694477807L;
    /**
     * Parser.
     */
    private static final DateTimeFormatter PARSER = new DateTimeFormatterBuilder()
        .appendValue(YEAR, 4, 10, SignStyle.EXCEEDS_PAD)
        .toFormatter();
}
```

于是你又总结了，私有的类常量，基本类型小驼峰，非基本类型使用全大写。但是你又错了。你看 `java.time.LocalDate`

```java
public final class LocalDate
        implements Temporal, TemporalAdjuster, ChronoLocalDate, Serializable {

    /**
     * The minimum supported {@code LocalDate}, '-999999999-01-01'.
     * This could be used by an application as a "far past" date.
     */
    public static final LocalDate MIN = LocalDate.of(Year.MIN_VALUE, 1, 1);
    /**
     * The maximum supported {@code LocalDate}, '+999999999-12-31'.
     * This could be used by an application as a "far future" date.
     */
    public static final LocalDate MAX = LocalDate.of(Year.MAX_VALUE, 12, 31);

    /**
     * Serialization version.
     */
    private static final long serialVersionUID = 2942565459149668126L;
    /**
     * The number of days in a 400 year cycle.
     */
    private static final int DAYS_PER_CYCLE = 146097;
}
```


