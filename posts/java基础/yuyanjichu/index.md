# Java语言基础


# Java语言基础

==声明==：下文可能涉及到一些词，他们是等价的。

- 成员属性 == 实例属性 == 成员变量
- 成员方法 == 实例方法 == 普通方法
- 类属性 == 类变量 == 静态变量
- 类方法 == 静态方法

## 概念

### 原码反码补码

| 数字                                                    | 原码      | 反码                                          | 补码                 |
| ------------------------------------------------------- | --------- | --------------------------------------------- | -------------------- |
| 10 = 0000 1010（整数原码就是其二进制表示）              | 0000 1010 | 0000 1010                                     | 0000 1010            |
| -10 = 1000 1010（负数的原码是其正数的源码，改变符号位） | 1000 1010 | 1111 0101（原码符号位不变，其余各位按位取反） | 1111 0110（反码加1） |

执行：-10 + 10

```sh
0000 1010
1111 0110
=
1 0000 0000
由于我们使用 1 字节存储，导致溢出的 1 被抛弃，最终结果就是 0
```

### 数据类型

注意：`java.lang.Void`是`void`的包装类，`null`对应的就是返回值`void`。

8 种基本数据类型，基本数据类型就是 CPU 可以直接运算的数据类型。其余是引用数据类型。但是 Java 的引用数据类型与 PHP 有区别，不是真引用，或者叫地址引用数据类型。

#### 大小

| 数据类型 | 字节 | 大小                                  | 定义                                                         | 默认值                              | 适用                    |
| -------- | ---- | ------------------------------------- | ------------------------------------------------------------ | ----------------------------------- | ----------------------- |
| byte     | 1    | -128（-2^7）~127（2^7-1）             | byte age = 100;                                              | 0                                   | 年龄，超过 127 岁的不多 |
| short    | 2    | -32768（-2^15）~32767（2^15-1）       | short student = 1000;                                        | 0                                   | 年级的学生              |
| int      | 4    | -2147483648(-2^31)~2147483647(2^31-1) | int people = 140000000;                                      | 0                                   | 中国人口 14 亿          |
| long     | 8    |                                       | long l1 = 0L; 或者<br />long l2 = 0l;                        | 0L<br />推荐 L ，小 l 容易跟 1 混淆 |                         |
| float    | 4    |                                       | float f1 = 0.0f; 或者<br />float f2 = 0.0F;                  | 0.0f                                |                         |
| double   | 8    |                                       | double d1 = 0.0d; 或者<br />double d2 = 0.0D; 或者<br />double d3 = 0.0; | 0.0d                                |                         |
| boolean  |      | ture 或 false                         |                                                              | false                               |                         |
| char     | 2    | 'A'                                   |                                                              |                                     |                         |

- 数据类型所占字节数

```ascii
       ┌───┐
  byte │   │
       └───┘
       ┌───┬───┐
 short │   │   │
       └───┴───┘
       ┌───┬───┬───┬───┐
   int │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
  long │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┬───┬───┐
 float │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
double │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┐
  char │   │   │
       └───┴───┘
```

#### boolean 占几字节

Java 语言对布尔类型的存储并没有做规定，因为理论上存储布尔类型只需要 1 bit，但是通常 JVM 内部会把 boolean 表示为 4 字节整数。

在符合 JVM 规范的虚拟机中，如果 boolean 是单独使用，boolean 占 4 个字节，如果 boolean 是以 “boolean数组” 的形式使用，boolean 占 1 个字节。

原因：

> 《Java虚拟机规范》一书中说 JVM 没有提供 booolean 类型专用的字节码指令，而是使用 int（4byte） 相关指令来代替。对 boolean 数组的访问与修改，会共用 byte（1byte） 数组的 baload 和 bastore 指令。

- 定义格式

4 种整型：

```java
// 整型：字节（-128 ~ 127），年龄一般不会超过 127 ，本着够用的原则
byte age = 23;
// 整型：短整型（-32768 ~ 32767），最大值 3 万 2
short studentNum = 31000;
// 整型：整型（-2147483648 ~ 2147483647），最大 21 亿，中国人口 14 亿
int chineseNum = 1400000000;
// 整型：长整型（-9223372036854775808 ~ 9223372036854775807），最大 9百万万亿，定义一个 23 亿
// 长整型，必须带标识 L，不推荐使用小 l ，因为跟 1 长得太像。ipv4 42 亿 9 千万
long ipv4 = 4294967296L;
assert ipv4 == (long) Math.pow(2, 32);
```

2 种浮点型，浮点数就是小数，由于使用科学计数法表示小数时，其小数点是可以前后浮动的，所以称之为浮点数。

```java
// 浮点类型单精度
float score1 = 59.3f;
float score2 = 59.3F;
// 默认是双精度浮点数
double score3 = 59.3;
```

浮点数的最大值

```java
System.out.println(Float.MAX_VALUE);
// 3.4028235E38 即 3.4*10^38
System.out.println(Double.MAX_VALUE);
// 1.7976931348623157E308 即 1.79*10^308
```

字符

```java
char var1 = 'a';
// Java 是 Unicode 编码，char 是 2 字节，所以支持中文字符
char var2 = '中';
```

布尔类型

```java
// 布尔类型
boolean res1 = false;
boolean res2 = !res1;
```

- 未初始化的变量，不能直接使用，但是有默认值

```java
// 年龄
byte age;
System.out.println(age);// java: 可能尚未初始化变量age
// 月薪
short salary;
System.out.println(salary);
// 中国人口
int chineseNum;
System.out.println(chineseNum);
// ipv4 最大个数
long ipv4;
System.out.println(ipv4);
float f;
double d;
// 状态
boolean isOK;
System.out.println(isOK);
// 字符
char a;
System.out.println(a);
```

- 常量

```java
// 常量使用，只能初始化一次：先定义，后初始化
final double PI;
PI = 3.14;
```

```java
// 常量使用，只能初始化一次：定义的同时进行初始化
final double PI = 3.14;
```

#### 数组

数组的 3 种定义方式：

```java
class HappyCode {
    @Test
    public void testDrivenLearn() {
        // 定义方式一：省略类型，常用
        char[] var1 = {'a', 'b', 'c'};
        System.out.println(var1);
        // 定义方式二：定义时指定大小
        char[] var2 = new char[3];
        var2[0] = 'a';
        var2[1] = 'b';
        var2[2] = 'c';
        System.out.println(var2);
        // 定义方式三：边定义边初始化，数组长度由编译器推算数组大小
        char[] var3 = new char[]{'a', 'b', 'c'};
        System.out.println(var3);
    }
}
```

### 浮点数不靠谱

- 浮点数存储不靠谱，运算也不靠谱

```java
// 十进制 0.1 的二进制是无限循环小数
System.out.println(0.1 + 0.2);
// 0.30000000000000004
```

- 浮点数的正确比较相等方式

```java
class HappyCode {
    @Test
    public void testDrivenLearn() {
        // 浮点数正确的比较方式
        double d1 = 0.3;
        double d2 = 0.1 + 0.2;
        // 错误的比较方式
        assert d1 != d2;
        // 正确的比较方式
        assert Math.abs(d1 - d2) < 0.00001;
    }
}
```

- 整数与浮点数一起运算发生类型提升

整型提升为浮点型

```java
class HappyCode {
    @Test
    public void testDrivenLearn() {
        // 类型提升，5，24 分别被提升为 5.0 和 24.0
        assert 4.8 == 24.0 / 5;
        assert 4.8 == 24 / 5.0;
        assert 4 == 24 / 5;
    }
}
```

### 除零溢出

```java
class HappyCode {
    @Test
    public void testDrivenLearn() {
        // java.lang.ArithmeticException: / by zero
        int var1 = 4 / 0;
    }
}
```

### 显示强转型

- 浮点型转成整型时，会丢弃小数部分，仅保留整数部分
- 如何让浮点型转整型做到四舍五入的效果，先给浮点数+0.5，在转型
- 不同类型运算时，低精度会提升为高精度，短会转成长的参与运算
- 比较两个浮点数通常比较它们的差的绝对值是否小于一个特定值

### 类文件

- 1个类文件里有且仅有1个`public`类，且类名与文件名保持一致
- 1个类文件里可以有多个`class`

### 方法签名

方法签名仅包含：方法名，形参（形参包括参数的个数，类型，顺序），==不包含方法返回值，和各种方法修饰符==

### 成员属性，成员方法

成员属性 == 实例属性 == 成员变量

成员方法 == 实例方法

成员属性，成员方法，在类实例化时初始化，即 new 的时候初始化，分配在堆中。

### 类属性，类方法

类属性 == 类变量 == 静态变量

类方法 == 静态方法

被`static`修饰的成员属性，叫做类属性。

被`static`修饰的成员方法，叫做类方法。

类属性和类方法在类加载的时候，初始化，分配在静态内存区，不需要做类实例化就能用。
### NO-OP

我们会发现一些接口的默认方法的 docs 中会出现 NO-OP 的描述。

大咖秀

```java
/**
  * Notification that an attribute has been added to a session. Called after
  * the attribute is added.
  * The default implementation is a NO-OP.
  *
  * @param se Information about the added attribute
  */
public default void attributeAdded(HttpSessionBindingEvent se) {
}
```

NO-OP == No Operation，即没有任何操作，没有操作不就是一个空方法体么。

### 继承

### 方法重写和重载

| 特点                                 | 方法重写                                       | 方法重载                                     |
| ------------------------------------ | ---------------------------------------------- | -------------------------------------------- |
| 定制修饰符                           | @Override                                      | 无                                           |
| 可修改方法修饰符（public\|final...） | 可以<br />子类重写方法的访问权限必须比父类大。 | 可以                                         |
| 变更方法返回值类型                   | 不可                                           | 可以                                         |
| 变更方法名称                         | 不可                                           | 不可                                         |
| 变更形参                             | 不可                                           | 可以<br />可以变更形参列表，个数，类型，顺序 |

==方法签名==不同即是方法重载，方法签名和方法返回值都相同，是重写。方法签名相同但方法返回值不同，编译器会报错。

加==@Override==注解，可以让编译器帮助检查是否进行了正确的重写。如果不小心写错了方法签名，编译器会报错。

PHP 子类继承父类的有参构造方法，而 Java 只能继承父类的无参构造。

```java

```

### 可见性

如下操作，会打开变量可见性。

| 操作                                 | 原因                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| 使用 ==volatile==修饰变量            |                                                              |
| 执行 ==synchronized== 修饰的代码     | 执行同步代码前，会重新加载                                   |
| 执行 ==Thread.sleep()== 方法让出 CPU | 让出 CPU 时，会重新加载变量                                  |
| 执行 ==System.out.println()== 方法   | println 是同步方法，因为要做打印字符和打印换行符 2 个操作<br />由于需要保证这 2 个操作同时执行，不发生线程切换，必须同步执行。 |

### 访问权限修饰符

![img](/images/024f78f0f736afc3d7954271b119ebc4b745120c.jpg)

## 规则

### 源文件规则

- 1 个源文件可以有多个非 public 类，但是只能有 1 个 public 类
- 源文件的名称要与 public 类的类名保持一致，包括大小写，因为 Java 大小写敏感

## JavaBean

### 特点

Java Bean 的特点，就是如果一个类满足如下特性，就可以说他是一个 Java Bean 了。

- 属性全私有，属性的访问和操作靠 setXXX/getXXX 方法
- 显式声明无参构造方法
- 类访问属性必须是 public class
- 实现 Serializable 接口，支持序列化

### 作用

> JavaBean 主要用来传递数据，即把一组数据组合成一个 JavaBean 便于传输。此外，JavaBean 可以方便地被 IDE 工具分析，生成读写属性的代码，主要用在图形界面的可视化设计中。
>
> JavaBean 是一种符合命名规范的`class`，它通过`getter`和`setter`来定义属性。
>
> 使用`Introspector.getBeanInfo()`可以获取属性列表。

JavaBean 细分为：DTO，VO

| Bean                                  | 作用 |
| ------------------------------------- | ---- |
| DAO（Data Access Object）数据访问对象 |      |
| VO（View Object）视图对象             |      |

## 常用类

### Serializable 接口

### 字符串

#### 编码

```java
class HappyCode {
    @Test
    public void testDrivenLearn() {
        char zh = '\u4e2d';
        assert '中' == zh;
        System.out.println(zh);
        // 中
    }
}
```

#### String

```java
class HappyCode {
    @Test
    public void testDrivenLearn() {
        String str = "hello world!";
        // 求字符串长度
        assert 12 == str.length();
        // 比较字符串是否相等
        assert str.equals("hello world!");
        // 获取二个字符，字符是基本数据类型，可以直接使用 == 比较
        assert 'e' == str.charAt(1);
        // 字符串转大写
        assert str.toUpperCase(Locale.ROOT).equals("HELLO WORLD!");
        // 判断字符串中是否包含字符 d
        assert true == str.contains("d");
        // 判断字符串是否以某个字串结尾
        assert true == str.endsWith("d!");
        assert true == str.startsWith("he");
        assert true == str.startsWith("ll", 2);
        // 截取字符串
        assert str.substring(0, 3).equals("hel");
        assert str.substring(2).equals("llo world!");
        // 查找字串的位置
        assert 2 == str.indexOf("l");
        assert 9 == str.lastIndexOf("l");
        assert 6 == str.indexOf("wo");
        // 空白判断
        assert true == "".isEmpty();
        assert false == " ".isEmpty();
        // 替换
        assert str.replace('l', 'L').equals("heLLo worLd!");
        // 字符数组，字符串互转
        char[] cs = "hello".toCharArray();
        cs[1] = 'w';
        String str1 = new String(cs);
        assert true == str1.equals("hwllo");
        cs[0] = 'w';
        assert true == new String(cs).equals("wwllo");
        assert true == str1.equals("hwllo");
    }
}
```

#### StringBuilder

#### StringJoiner

```java
class HappyCode {
    @Test
    public void testDrivenLearn() {
        // 字符串拼接
        String[] names = {"tony", "maria",};
        assert true == "tony,maria".equals(String.join(",", names));
        StringJoiner stringJoiner = new StringJoiner(",", "hello ", "!");
        for (String name : names) {
            stringJoiner.add(name);
        }
        assert true == "hello tony,maria!".equals(stringJoiner.toString());
    }
}
```

总结：拼接需要使用前后缀的，使用`StringJoiner`，只需要简单的拼接使用`java.lang.String#join(java.lang.CharSequence, java.lang.CharSequence...)`。

#### StringBuffer

`StringBuilder`的线程安全版，原因使用`synchronized`修饰了方法。

```java
public synchronized StringBuffer append(String str);
public synchronized StringBuffer append(StringBuffer sb);
public synchronized String substring(int start);
// ...
```

### 数组

#### 数组`[]`和`Array`，`ArrayList`区别

- `[]`是定义数组简写
- `Array`这个类不存在，即不存在`java.lang.Array`
- `ArrayList`是集合，是`List`的子类，类似数组

#### 数组`[]`转`List`

### Map

#### HashMap

#### HashTable

HashTable 相较于 HashMap 线程安全。

### 任意大小的数

#### BigInteger-任意大小的整数

#### BigDecimal-任意大小的小数

`BigDecimal`可以表示一个任意大小且精度完全准确的浮点数。

| 方法                                                         | 特点                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ==                                                           | 比较两个对象的地址，即是否为同一个对象                       |
| java.math.BigDecimal#compareTo<br />相等返回 0，大于返回 1，小于返回 -1 | 仅比较两个数的大小，不比较精度`scale`<br />比较两个数是否相等，==推荐使用compareTo()方法== |
| java.math.BigDecimal#equals                                  | 不仅比较两个数的大小，还比较精度                             |

打开`BigDecimal`源码，发现其包含一个大整数和小数的位置。

![image-20220905210834841](/images/image-20220905210834841.png)

### Math

#### 随机数生成

```java
// 生成随机数[min, max)：随机比例 * (max - min) + min，生成1，100的随机数
double random1 = Math.random() * (101 - 1) + 1;
int random2 = (int) random1;
```

`Math.random()`返回值介于`[0, 1)`之间，类似一个比例。

### ReentrantLock

## 装箱

- 所有的包装类型都是不变的，因为其底层都是基本类型常量`private final float value;`
- 自动装箱，自动拆箱在编译期完成
- 包装类型比较必须使用`equals()`方法

```java
class HappyCode {
    @Test
    public void testDrivenLearn() {
        // 隐式装箱
        Integer age1 = 23;
        // 显式装箱，常用隐式装箱
        Integer age2 = new Integer(23);
        Integer age3 = Integer.valueOf(23);
        // 显式拆箱
        int age4 = age1.intValue();
        // 隐式拆箱，常用隐式拆箱
        int age5 = age2;
    }
}
```

### BigDecimal

#### 安全使用 2 种方式

todo

## 类型转换

### 字符串转其他

| 操作                 | code                                                         |
| -------------------- | ------------------------------------------------------------ |
| 转字节类型           | String s = new String("hello 广州");<br />byte[] sBytes = s.getBytes(StandardCharsets.UTF_8); |
| 基本数据类型转字符串 | String.valueOf() 重载了支持多种基本数据类型<br />![image-20220915190539050](/images/image-20220915190539050.png) |

### 整型转其他

## 多态

> 多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。

```java
class Tony implements Student {
    @Override
    public void say() {
        System.out.println("I am tony");
    }
}

class Maria implements Student {
    @Override
    public void say() {
        System.out.println("I am maria");
    }
}

class People {
    public static void introduce(Student student) {
        student.say();
    }
}

class HappyCode {
    @Test
    public void testDrivenLearn() {
        People.introduce(new Tony());
        // I am tony
        People.introduce(new Maria());
        // I am maria
    }
}
```

## Object类

Object 是 Java 中的万类之父，类似 PHP 中的 `stdClass`。

### hashCode与equals

集合需要检查元素是否存在，hashCode 获得元素地址，比遍历元素比较 equals 来的快。

A.equals(B) == true 即两个值相同，其 hashCode 一定相同。但是反过来，hashCode 相同的，其值不一定相同。

- equals 相同，hashCode 必须相同，要求重写 equals 的同时==必须重写 hashCode()==
- hashCode 相同，equals 不一定相同。说明存在==哈希冲突==，相同 hashCode 但是 equals 不同的元素，我们在同一个哈希地址上，用链表存储，检查 HashMap 上是否存在某个值时，首先计算对象的 Hash 值，找到哈希地址，O(1)，发现此哈希地址上挂载了链表，此位置存在哈希冲突，然后遍历链表逐个元素比较，O(n)。

例如：

9 个哈希值相同的英文字符串：

```java
ArrayList<Integer> arrayList = new ArrayList<>();
arrayList.add("3Qj".hashCode());
arrayList.add("2pj".hashCode());
arrayList.add("2qK".hashCode());
arrayList.add("2r,".hashCode());
arrayList.add("3RK".hashCode());
arrayList.add("3S,".hashCode());
arrayList.add("42j".hashCode());
arrayList.add("43K".hashCode());
arrayList.add("44,".hashCode());
System.out.println(arrayList);
// [51628, 51628, 51628, 51628, 51628, 51628, 51628, 51628, 51628]
```

2 个哈希值相同的中文词：

```java
```

- equals() 方法的 4 大原则，指导我们实现 equals 方法必须遵循这个原则

| 特性   | equals 方法必须遵循以下原则                                  |
| ------ | ------------------------------------------------------------ |
| 自反性 | A.equals(A) == true                                          |
| 一致性 | 没有修改 A 和 B 的情况下，永久 true == A.equals(B)<br />像不像幂等 |
| 传递性 | 如果 true == A.quals(B)，true == B.equals(C)，存在 A.equals(C) |
| 对称性 | 如果 true == A.equals(B)，存在 true == B.equals(A)           |

## Class类

Class是一个用来表示类的类，它内部可以记录类的成员、接口等信息。

Class 类也是 Object 的子类。

获取类的 Class 实例的 3 种方法

1. 通过当前类对象的 getClass() 方法获取 Class 的实例
2. 通过 Class.forName(当前类的完整限定名) 获取当前类的 Class 实例
3. 当前类`类名.class`获取 Class 实例

A.class 即 A 类的表达类 Class 的实例。在 JVM 种，每个类的 Class 类有且仅有一个实例。
或者说 A.class 是  new A() 实例的模板。

## 构造方法

```java
class Parent {
}
// 下面是编译器编译生成的字节码
class Parent {
    Parent() { /* compiled code */ }
}
```

- javac 编译器一定会为无构造方法的类，创建无参构造方法，并在所有构造方法的第一行隐式调用其父类的无参构造；定义了构造方法的类，编译器不再为其创建无参构造方法
- 子类所有构造方法的第一行，必须是调用父类的构造方法，父类的无参构造可以隐式调用不写，有参构造必须显示调用写出来
- 子类不可能重写父类的构造方法，即使子类中定义了一个与父类同名的方法，那也不叫重写，只是一个新方法

```java
class Parent {
    public Parent() {
        System.out.println("父类的无参构造");
    }
}

class Son1 extends Parent {
    public void Parent() {
        System.out.println("子类的普通方法");
    }

    public Son1() {
        System.out.println("子类的无参构造");
    }
}

new Parent();
// 父类的无参构造
new Son1();
// 父类的无参构造 子类的无参构造
```

## 初始化顺序

静态变量和静态代码块，在类加载时初始化，==至于静态变量和静态代码块初始化的顺序，取决于他们定义的顺序==。

实例变量和实例方法在类加载后，new 的时候初始化。

## 静态导包

即一次性导入某个类的所有静态方法和静态属性，在本类中使用导入类的公有，私有属性和方法，不用加类名，就像使用本类的属性和方法一样容易。

有点类似 PHP 里的 `use triat`，在本类中，复制了一份导入类的代码。

## 宏

其实 C 语言的宏定义在 Java 中是真实存在的。由于低版本的 PHP 还不支持类属性被赋值表达式，进而无法完成宏定义。

```java
class Chinese {
    public static final Chinese HELLO = new Chinese().hello();

    public Chinese hello() {
        // 宏定义任务
        System.out.println("hello world");
        return this;
    }
}

// 执行宏
Chinese hello = Chinese.HELLO;
```

### 大咖秀

```java
public final class Boolean implements java.io.Serializable,
                                      Comparable<Boolean>
{
    /**
     * The {@code Boolean} object corresponding to the primitive
     * value {@code true}.
     */
    public static final Boolean TRUE = new Boolean(true);

    /**
     * The {@code Boolean} object corresponding to the primitive
     * value {@code false}.
     */
    public static final Boolean FALSE = new Boolean(false);
}

// 利用宏定义 new 一个对象
Boolean abc = Boolean.TRUE;
```

## 静态方法

- 静态方法不能是抽象的，因为他要在类加载的时候执行方法体

## 内部类

内部静态类有静态方法，非静态方法

内部非静态类只有非静态方法

```java
class OutClass {
    class InnerClass {
        public void say() {
            System.out.println("我是普通内部类普通方法");
        }

        // java 8 不支持普通内部类中定义静态方法
        // public static void staticSay() {
        //     System.out.println("我是普通内部类的静态方法");
        // }
    }

    static class StaticInnerClass {
        public void say() {
            System.out.println("我是静态内部类的普通方法");
        }

        public static void staticSay() {
            System.out.println("我是静态内部类的静态方法");
        }
    }

    public void say() {
        System.out.println("我是外部类的普通方法");
    }
}

// 调用内部静态类的，静态方法
OutClass.StaticInnerClass.staticSay();
OutClass outClass = new OutClass();
// 调用内部非静态类的，非静态方法
OutClass.InnerClass innerClass = outClass.new InnerClass();
innerClass.say();
// 那么如何调用内部静态类的，非静态方法呢
```

## 关键字

### switch

switch(变量) 不支持 Long/long 类型。

### 模板类Class

Class 类的实例是每个类的实例的模板，且 JVM 中有且仅有一份。

代码展示：

```java
class Student {
}

// 由于一个类有且仅有一个对象模板，所以下面三个 Class 对象是相同的
Student student1 = new Student();
Student student2 = new Student();
assert student1 != student2;
// 获取类的模板实例方式一 == 获取类的模板实例方式二
assert Student.class == student1.getClass();
assert Student.class == student2.getClass();
assert student1.getClass() == student2.getClass();
// Object 是万类之父?
assert Student.class instanceof Class;
assert Student.class instanceof Object;
assert Object.class instanceof Class;
assert Class.class instanceof Object;
// 不可比较的类型: java.lang.Class<java.lang.Class>和java.lang.Class<java.lang.Object>
System.out.println(Class.class);
// class java.lang.Class
System.out.println(Object.class);
// class java.lang.Object
assert Class.class instanceof Class;
assert Class.class.getClass() instanceof Class;
assert Class.class == Class.class.getClass();
// 问 Object 跟 Class 的关系?
// 必须捕获异常
try {
    // 获取类的模板实例：方式三
    assert Class.forName("shali.bbs.temp.Student") instanceof Class;
} catch (ClassNotFoundException e) {
    e.printStackTrace();
}
```

### Class<?>

==不要觉得 Class 类多么高端莫测，其实他就相当于 PHP 中的`ReflectionClass`类，对反射类。不知道 Java 为何不叫反射类。==

```java
public final native Class<?> getClass();
```

说`?`表示未知的类型，既然是未知的类型，为何不像泛型那样，使用`Class<T>`呢？

Class 类的实例是单例

```java
import org.junit.Assert;
import org.junit.Test;

public class ClassTest {
    class A {
    }

    @Test
    public void singleton() {
        A a1 = new A();
        A a2 = new A();
        Assert.assertNotSame(a1, a2);
        Class<? extends A> a1Class = a1.getClass();
        Class<? extends A> a2Class = a2.getClass();
        Class<A> a3Class = A.class;
        Assert.assertSame(a1Class, a2Class);
        Assert.assertSame(a1Class, a3Class);
    }
}
```

### volatile

作用：==打开线程间变量的可访问性==，==禁止指令重排序==。

英文的意思是很容易变的，需要你常去更新。

大咖秀

```java
public class Thread implements Runnable {
    private volatile String name;
}
```

volatile 修饰的变量，线程间可访问。线程的名字用 volatile 修饰，就是为了让不同的线程，可以获取彼此的名称。

volatile 修饰的线程共享变量，存放在主内存区。

JMM（Java　Memory Model），Java 内存模型规定：

所有的共享变量都存储于主内存。这里所说的变量指的是实例变量和类变量，不包含局部变量，因为局部变量是线程私有的，因此不存在竞争问题。
各个线程自己的变量存放在本地内存。

思考开放线程中的变量的可访问性，还有什么方法？

- volatile 同 synchronized 对比

#### 指令重排

注意下面的代码不是重排序，而是 JVM 的优化。

```java
// 源码
boolean isStop = false;
while (!isStop) {}

// JVM 指令重排
while (true) {}
```

### extends

- 接口 extends 接口

- 抽象类 extends 接口
- 抽象类 extends 抽象类
- 具体类 extends 接口
- 具体类 extends 抽象类

不像其他语言，对泛化和实现关系使用不同的关键字进行区分，比如 PHP，`extends`表泛化，`implements`表实现。

### transient

adj. 短暂的，临时的

我们知道，如果想序列化一个对象，只要其实现了`java.io.Serializable`接口，其属性和方法就能被自动地序列化。但是有些属性比较敏感，比如用户的密码属性，我们只想其运行时出现在内存中，不想被序列化传输和保存怎么办，那我们就需要给这个属性进行标记。

```java
private transient String password;
```

- transient 只能修饰成员属性，不能修饰类属性，因为类属性，修饰与否，都不会被序列化
- transient 一旦修饰了某个成员变量，这个实例被反序列化后，其成员变量是 null

### native

```java
public native int hashCode();
```

被`native`修饰的方法，说明这个方法不是 Java 语言实现，而是其他语言编译生成动态链接库`dll`给 Java 调用。

### final

| 被修饰者 | 特性                                                         |
| -------- | ------------------------------------------------------------ |
| 局部变量 | 被 final 修饰的局部变量是局部常量，只能赋值一次，不可改变<br />==final 修饰局部常量，使用之前必须赋值== |
| 成员属性 | 被 final 修饰的成员属性叫实例常量<br />==final 修饰的成员变量声明时未赋值的，必须在构造方法/静态代码块中初始化，即保证创建对象的时候初始化。== |
| 类属性   | 类常量                                                       |
| 成员方法 | 不能被重写Override，==可以被重载==                           |
| 类       | 不能被继承，final 让类变成太监类                             |

- final 修饰成员属性

```java
class People {
    // 一：定义的时候初始化
    private final String nationality = "汉";

    // 二：静态代码块里初始化
    private static final String country;

    // 三：构造方法里初始化
    private final String name;

    static {
        country = "中国";
    }

    People(String name) {
        this.name = name;
    }
}
```

### static

| 被修饰者 | 特点                                                         |
| -------- | ------------------------------------------------------------ |
| 代码块{} | 被 static 修饰的代码块，在类文件加载时，就执行。             |
| 类属性   | 被 static 修饰的成员属性就会变成类属性，因为所有对象共享此变量 |
| 类方法   | 被 static 修饰的成员方法为类方法，类方法，可以不需要实例化，直接使用类名调用。<br />类方法中不能调用成员方法，但是成员方法中可以调用类方法。 |

### synchronized

`synchronized`翻译成同步，很容易产生误解。

因为汉语中的同步是同时进行，同时发生。而`synchronized`刚好相反，不能同时进行，要按照顺序进行。

那线程同步是不是要让两个线程同时执行呢？

不是的，这里的`synchronized`同步就不是那个意思了，而是我前面做，你后面做，我们不能同时做。即方法 A 不执行完，方法 B 不能执行。

`异步`就不一样了，方法 B 不需要等方法 A 执行完，就可以去执行，这是异步。

| 被修饰者 | 特点                                                         |
| -------- | ------------------------------------------------------------ |
| 抽象方法 | synchronized 不能修饰抽象类或接口中的抽象方法                |
| 构造方法 | synchronized 不能修饰构造方法                                |
| 普通方法 | synchronized 修饰的构造方法，子类重写时，如果需要同步，需要继续添加 synchronized 关键字 |

#### 同步代码块

```java
```

#### 同步方法

```java
```

同步代码块和同步方法，其实都说明了其持有的锁。只不过同步方法隐式地持有 this 对象。

注意，当同步代码（同步代码块和同步方法）使用相同的锁时，彼此不会等对方是否锁，即可进入同步代码。

> 由于 synchronized 是在 JVM 层面实现的，因此系统可以监控锁的释放与否；
> 而 ReentrantLock 是使用代码实现的，系统无法自动释放锁，需要在代码中的 finally 子句中显式释放锁 lock.unlock()。
>
> 另外，在并发量比较小的情况下，使用 synchronized 是个不错的选择；
> 但是在并发量比较高的情况下，其性能下降会很严重，此时 ReentrantLock 是个不错的方案。

## 注解

Java1.5 开始引入注解。

| 注解        | 类型         | 功能描述                                                     |
| ----------- | ------------ | ------------------------------------------------------------ |
| @Override   | 语言内置注解 | 注解方法，说这是一个被子类重写的方法                         |
| @Target     | 元注解       | 注解注解，说这个注解可以用来注解类还是类方法<br />ElementType.METHOD 表示可以用来注解类方法<br />ElementType.TYPE 表示可以用来注解类 |
| @Retention  | 元注解       | 注解注解，用于提示注解被保留多长时间。<br />例如 RetentionPolicy.RUNTIME 表示注解在运行期间一直保留 |
| @Documented | 元注解       | 表示注解是否能被 javadoc 处理并保留在文档中                  |

### 元注解

元注解就是能够定义注解的注解。我们可以使用元注解，来定制我们自己需要的注解。

### 自定义注解

下面我们要实现自己的注解。

#### 学习元注解

可以修饰注解的注解，我们称之为元注解。

| 关键字      | 功能描述                                                     |
| ----------- | ------------------------------------------------------------ |
| @interface  | ==不是元注解==，注解类型声明关键字，类似`class`，`interface`等声明关键字 |
| @Inherited  | 子类是否能从被父类继承此注解                                 |
| @Target     | 参数是`ElementType[]`，指定被注解修饰的类型                  |
| @Documented | 此注解是否允许被 Javadoc 工具解析成文档                      |
| @Retention  | 指明注解的声明周期，存于源码中，字节码中，还是 JVM 运行时。  |

#### 定义注解

我们希望定义一种注解，当其注解方法时，就输出一句话。输出的话可以指定内容。

定义注解也可以理解为定义标签。

```java
import java.lang.annotation.*;

// 注解允许被 Javadoc 工具提取为文档
@Documented
// 注解允许被继承
@Inherited
// 仅注解方法
@Target({ElementType.METHOD})
// 声明周期到运行时
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    // 可以理解为注解的属性，写成方法的形式是为了后面好调用
    String say() default "";
}
```

#### 使用注解

```java
import java.lang.reflect.Method;

public class AnnotationTest {
    @Test
    @MyAnnotation(say = "我的注解解析器被执行了")
    public void annotationProcessor() {
        Method method = null;
        try {
            // 反射当前方法
            method = this.getClass().getMethod("annotationProcessor");
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        }
        if (null == method) {
            return;
        }
        // 检查当前方法是否存在我定义的注解
        MyAnnotation myAnnotation = method.getAnnotation(MyAnnotation.class);
        if (null != myAnnotation) {
            System.out.println(myAnnotation.say());
        }
    }
}
```

起始我们上面的代码不仅使用了注解，还解析了注解。但是这种解析是方法自己完成的，能不能交给容器来完成我们自定义注解的处理呢。

#### 定义注解处理器

```java
import javax.annotation.processing.AbstractProcessor;

@SupportedAnnotationTypes("com.shali.ruby.bean.MyAnnotation")
public class MyAnnotationProcessor extends AbstractProcessor {
    @Override
    public synchronized void init(ProcessingEnvironment processingEnv) {
        super.init(processingEnv);
    }

    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
        return false;
    }
}
```

## 重写Override方法

### 重写equals()必重写hashCode()方法

由于我们要保证相同的对象，其哈希值必定相同。

- 定义一个中国公民类，只要他们身份证号相同，我们就认为他们是同一个人

```java
class People {
    private final String id;

    People(String id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object o) {
		// 不重写 equals 方法，就没法做到，相同身份证 id，用 equals() 判断是同一个人
        if (this == o) {
            return true;
        } else if (o == null || getClass() != o.getClass()) {
            return false;
        }
        People people = (People) o;
        return this.id.equals(people.id);
    }
}

class HappyCode {
    @Test
    public void testDrivenLearn() {
        People people1 = new People("123456");
        People people2 = new People("123456");
        assert people1.equals(people2);
        assert people1.hashCode() != people2.hashCode();
        System.out.println(people1.hashCode());
        // 22805895
        System.out.println(people2.hashCode());
        // 1413378318
    }
}
```

通过我们重写`equals()`方法，仅比较 id ，我们做到了相同 id，就是相同的人。但是我们发现相同对象，但是其 hash 值却不相同。这有点违背`equals()`相同的对象，其哈希值必相同的原则。看来我们需要重写`hashCode()`方法。

```java
class People {
    private final String id;

    People(String id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object o) {
        // 不重写 equals 方法，就没法做到，相同身份证 id，用 equals() 判断是同一个人
        if (this == o) {
            return true;
        } else if (o == null || getClass() != o.getClass()) {
            return false;
        }
        People people = (People) o;
        return this.id.equals(people.id);
    }

    @Override
    public int hashCode() {
        // 重写了 equals 方法，必重写 hashCode 方法，不然没法确保 equals 相同，hashCode 必相同的原则
        return this.id.hashCode();
    }
}

class HappyCode {
    @Test
    public void testDrivenLearn() {
        People people1 = new People("123456");
        People people2 = new People("123456");
        assert people1.equals(people2);
        assert people1.hashCode() == people2.hashCode();
        System.out.println(people1.hashCode());
        // 1450575459
        System.out.println(people2.hashCode());
        // 1450575459
    }
}
```

## 泛型与通配符?的区别

- 通配符 ? 表示编译期不能确定的类型，但运行时其类型是确定的
- T，K，V，E，A，B，C 泛型代表编译时其类型就是确定的，因为编码使用的时候已经指的了类型

## 黑窗口

黑窗口，命令行运行代码

编译 `javac -encoding utf8 Test.java`

运行并开启断言 `java -ea Test`，如果不开启断言，断言失败了，是不会抛出异常的。

## 线程

常用方法：

| 方法     | 特点                                                         |
| -------- | ------------------------------------------------------------ |
| wait()   | 释放对象锁<br />让出 CPU<br />需要 notify() 唤醒，才能使用 CPU |
| sleep()  | 不释放对象锁<br />让出 CPU<br />不需要任何唤醒，睡眠时间到，就可以去使用 CPU |
| notify() | 执行完 notify() 方法，当前线程并不会释放锁，需要等 synchronized 代码执行完毕 |

wait 让出 CPU，释放锁，暂停当前线程，后续需要唤醒，才能被 CPU 调度。

yield 类方法，让出 CPU，不释放锁，类似循环中的 continue，迎接下一轮的 CPU 调度。

sleep 类方法，让出 CPU，不释放锁，类似定时器，定时时间到了，才能被 CPU 调度。

interrupt 无实质作用的方法。

### 线程空间

| 空间                             | 特点                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| 主内存                           | 线程之间的共享变量（实例变量和类变量，不包含局部变量）存储在主内存中。<br />局部变量是线程私有的，是不存在竞争问题。我们平常上锁都是给共享变量上锁。 |
| 线程私有的本地内存，也叫工作内存 | 本地内存中存储了该线程读/写共享变量的副本。<br />线程不能直接读写共享变量，必须借助工作内存。 |

### 线程内存模型

JMM（Java Memory Model）的抽象示意图：

![img](/images/v2-3d312429710bd6a11eca171858f67751_720w.jpg)

### 线程变量可见性

| 方式         | 特点                                                         |
| ------------ | ------------------------------------------------------------ |
| volatile     | 使用 volatile 修饰共享变量后，每个线程要操作变量时会从主内存中将变量拷贝到本地内存作为副本，当线程操作变量副本并写回主内存后，会通过 ==CPU 总线嗅探机制==告知其他线程该变量副本已经失效，需要重新从主内存中读取。 |
| synchronized | 当一个线程进入 synchronized 代码块后，线程获取到锁，会清空本地内存，然后从主内存中拷贝共享变量的最新值到本地内存作为副本，执行代码，又将修改后的副本值刷新到主内存中，最后线程释放锁。 |

## 池

### 字符串常量池

```java
String s1 = "hi";
String s2 = "hi";
String s3 = new String("hi");
String s4 = new String("hi");
assert s1 == s2;
assert s3 != s2;
assert s3 != s4;
String s5 = s3.intern();
String s6 = s4.intern();
assert s5 == s6;
assert s1 == s5;
assert s5 != s3;
```

### 缓存池

- Integer.valueOf() 强制使用缓存池

下面是此方法的实现源码：

```java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```

在 Java 8 中，Integer 缓存池的大小默认为 -128 ~ 127。因为`IntegerCache.low`是`-128`，`IntegerCache.high`是`127`。

其实仔细看源码发现，最大值介于`java.lang.Integer.IntegerCache.high`~`Integer.MAX_VALUE - (-low) -1`之间，`java.lang.Integer.IntegerCache.high`是配置。

下面用代码来验证。

```java
Integer a1 = 127;
Integer a2 = 127;
Integer a3 = 128;
Integer a4 = 128;
assert a1 == a2;
assert a3 != a4;
Integer a5 = -128;
Integer a6 = -128;
Integer a7 = -129;
Integer a8 = -129;
assert a5 == a6;
assert a7 != a8;
```

那么我们如何使我们定义的变量，用到缓存池呢？

答案是使用装箱方法，不是 new。看代码：

```java
// 定义一个缓存池常量
Integer a1 = Integer.valueOf(123);
Integer a2 = new Integer(123);
Integer a3 = 123;
// a1 在缓存池，a2 在堆中
assert a1 != a2;
// a3 会发生自动装箱操作，即调用 Integer.valueOf 方法，就是利用缓冲池
assert a3 == a1;
```

## 锁

互斥锁（mutex）只有加锁、解锁两种操作，且互斥锁的释放只能由加锁的那个线程来释放。

读写锁：

某线程对资源加读锁时，其他线程可以也可以加读锁，但是不能加写锁（意味着有人在读的时候，其他人也能读，不能写）。
某线程对资源加写锁时，其他线程什么锁都不能加（意味着有人在写的时候，其他人什么都不能干）。

读写锁有加读锁、加写锁、解锁三种操作。

排他锁：等于读写锁中的写锁，因为其他人什么都不能干，所以“排他”。
共享锁：等于读写锁中的读锁，因为其他人还可以读，所以“共享”。

自旋锁：线程在获取共享资源前先申请获取锁，如果申请不到，则一直继续申请（而不是阻塞），直到申请到为止。

循环自旋锁，递归自旋锁。

悲观锁：行锁，表锁。

乐观锁：

CAS 机制：Compare And Swap 比较并替换，即修改之前现检查。

### synchronized

### ReentrantLock

### 总结

synchronized：加锁解锁过程由 JVM 自动控制

ReentrantLock：需要手动释放锁 unlock

> 最后再贴一段 Doug Lea 大神在书中，关于在 synchronized 和 ReentrantLock 之间进行选择的原话，该如何选择，读者自己去思考吧。
>
> 
>
> 在一些内置锁无法满足需求的情况下，ReentrantLock 可以作为一种高级工具。当需要一些高级功能时才应该使用 ReentrantLock，这些功能包括：可定时的，可轮询的与可中断的锁获取操作，公平队列，以及非块结构的锁。否则，还是应该优先使用synchronized。

## 新特性

### 1.8

#### Optional

Optional 类是 Java8 的新特性，是一个可以为 null 的容器对象。用来解决空指针异常（NPE：java.lang.NullPointerException）。

```java
UserInfo userInfo = new UserInfo();
userInfo.setAddress("guangzhou");
User user = new User();
user.setUserInfo(userInfo);
user = null;
// 假如我们要获取 user 的地址信息，就要战战兢兢，防止 npe
// 报空指针异常
System.out.println(user.getUserInfo());
// if else 检查，不报空指针异常
if (null != user) {
    UserInfo userInfo1 = user.getUserInfo();
    if (null != userInfo1) {
        String address = userInfo1.getAddress();
        System.out.println(address);
    }
}
// Optional 容器处理不报 npe
String address = Optional.ofNullable(user).map(u -> u.getUserInfo()).map(info -> info.getAddress()).orElse(null);
```

顺便提一下`PHP8.0`也增加了类似的语法糖。

```php
// 中途有一个实例是 null，则返回给 $address 的是 null
$address = user?->getUserInfo()?->getAddress();
```

印象中`Swift`语言也有此类特性，暂不记得，后面想起再补充。

#### 接口支持定义抽象方法的默认实现

大咖秀

```java
package javax.servlet.http;

import java.util.EventListener;

public interface HttpSessionListener extends EventListener {
    public default void sessionCreated(HttpSessionEvent se) {
    }

    public default void sessionDestroyed(HttpSessionEvent se) {
    }
}
```

#### 支持函数式编程，Lambda

> 函数式编程最早是数学家阿隆佐·邱奇研究的一套函数变换逻辑，又称Lambda Calculus（λ-Calculus），所以也经常把函数式编程称为Lambda计算。

#### Date and Time API

#### Pipelines and Streams

#### Type Annotations

#### Nashhorn JavaScript Engine

#### Concurrent Accumulators

## 解惑

### 字符串

- 为什么 StringBuffer 相对于 StringBuilder 线程安全？

因为`java.lang.StringBuffer#append(java.lang.String)`方法使用了同步方法。

```java
@Override
public synchronized StringBuffer append(String str) {
    toStringCache = null;
    super.append(str);
    return this;
}
```

- 为什么 String 是不可变的？

因为 String 类的底层使用`char[]`来存储的字符串的字符元素，而且这个字符数组被`final`修饰，即常量数组。

```java
private final char value[];
```

### 哈希表

- 为什么 Hashtable 相对于 HashMap 是线程安全的？

因为`java.util.Hashtable#put`方法被`synchronized`修饰，即同步方法。

```java
public synchronized V put(K key, V value) {}
```

### Serializable 接口

- 为何实现 Serializable 接口时，要重写`serialVersionUID`属性？

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    private static final long serialVersionUID = -6849794470754667710L;
}
```

`serialVersionUID`其实是用在反序列化时，验证类文件版本的一致性。

## 引用

- [volatile 关键字，你真的理解吗？](https://zhuanlan.zhihu.com/p/138819184)

