# Java反射应用


反射真是无所不能，不仅能修改类常量，还能修改对象属性的访问权限。

## 功能

反射有如下功能。

1. 修改类常量，但是**不能修改被基本类型类常量**
2. 引用类型类常量也不是全能修改，比如**被字符串常量池初始化的字符串类常量，反射也不能修改**。
3. 修改对象属性的访问权限

### 代码验证

```java
// AppTest.java

@Getter
class User {
    // 被字符串常量池初始化的字符串常量，不可被反射修改
    public final String name1 = "maria";
    // 反射可修改
    public final String name2 = new String("maria");
    // 反射可修改
    public final String name3 = null;
    // 基本类型常量，反射不可修改
    public final int age1 = 23;
    // 基本类型常量，反射可修改
    public final int age2 = Integer.valueOf(23);
    // 被整型常量池初始化的整型常量，可被反射修改
    public final Integer age3 = Integer.valueOf(23);
    // 整型常量，反射不可修改
    public final Integer age4 = 23;
    // 反射不可改
    public final char sex1 = 'm';
    // 反射可修改
    public final char sex2 = Character.valueOf('m');
    // 反射可修改
    public final Character sex3 = 'm';
    // 反射可修改
    public final Character sex4 = Character.valueOf('m');
}

public class AppTest {
    @Test
    public void testReflect() throws NoSuchFieldException, IllegalAccessException {
        User user = new User();
        Class<? extends User> aClass = user.getClass();
        // 反射无法修改字符串常量池
        Field name1 = aClass.getDeclaredField("name1");
        name1.setAccessible(true);
        name1.set(user, "tom");
        Assert.assertEquals("maria", user.getName1());
        // 反射可修改非常量池初始化的字符串常量
        Field name2 = aClass.getDeclaredField("name2");
        name2.setAccessible(true);
        name2.set(user, "tom");
        Assert.assertEquals("tom", user.getName2());
        // 反射可修改被 null 初始化的常量
        Field name3 = aClass.getDeclaredField("name3");
        name3.setAccessible(true);
        name3.set(user, "tom");
        Assert.assertEquals("tom", user.getName3());
        // 反射不可修改基本类型常量
        Field age1 = aClass.getDeclaredField("age1");
        age1.setAccessible(true);
        age1.set(user, 24);
        Assert.assertEquals(23, user.getAge1());
        // 反射可以修改整型常量池初始化的常量
        Field age2 = aClass.getDeclaredField("age2");
        age2.setAccessible(true);
        age2.set(user, 24);
        Assert.assertEquals(24, user.getAge2());
        // 反射不可修改整型常量池初始化的基本类型常量
        Field age3 = aClass.getDeclaredField("age3");
        age3.setAccessible(true);
        age3.set(user, 24);
        Assert.assertEquals(24, (int) user.getAge3());
        // 可修改
        Field age4 = aClass.getDeclaredField("age4");
        age3.setAccessible(true);
        age3.set(user, 24);
        Assert.assertEquals(23, (int) user.getAge4());
    }
}
```

看了是不是晕了，我来总结下。

1. 直接赋值的，反射无法修改
2. `public final Character sex3 = 'm';` 是个特例

## ChatGPT有话说

> 问：Java反射能修改被基本类型常量吗
>
> 答：在 Java 中，基本类型的变量是按值传递的，而不是按引用传递的。基本类型的常量是不可变的，它们的值在初始化后不能被修改。因此，Java 的反射机制无法直接修改基本类型的常量。
>
> 反射机制主要用于操作类的结构和对象的属性，包括访问和修改对象的字段、方法和构造函数等。但基本类型的常量不属于对象的属性，因此无法通过反射来修改它们的值。
>
> 如果你想修改一个常量的值，它应该是一个引用类型的常量，而不是基本类型的常量。对于引用类型的常量，可以使用反射来修改它们的属性值。但需要注意的是，修改常量的值可能会违反常量的定义和设计初衷，因此应该谨慎使用反射来修改常量的值。
>
> 问：Java反射能修改被字符串常量池初始化的字符串常量吗
>
> 答：在 Java 中，字符串常量池是一个特殊的内存区域，用于存储字符串常量。字符串常量池中的字符串是不可变的，一旦被初始化，它们的值不能被修改。
>
> 由于字符串常量池的特性，Java 的反射机制无法直接修改被字符串常量池初始化的字符串常量的值。无论是通过反射访问字段还是调用方法，都无法改变字符串常量的值。
>
> 需要注意的是，通过反射机制修改字符串常量的值是不被推荐的做法，因为字符串常量的不可变性是 Java 语言设计的一部分，它具有重要的语言特性和性能优化。修改字符串常量的值可能会导致意料之外的行为，并且违背了字符串常量的设计初衷。
>
> 如果你需要修改字符串的内容，可以使用可变的字符串类（如 `StringBuilder` 或 `StringBuffer`）来操作字符串，并根据需要进行修改。
