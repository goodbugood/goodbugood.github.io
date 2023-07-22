# 地址，指针与引用


## 区别

| 概念   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| 指针   | 变量在内存中的地址，C，C++ 在使用，再高级点的语言，抛弃了，因为管理内存地址很麻烦。 |
| 传值   | 传值很简单，就是将其他变量的值拷贝一个副本给本变量。<br />需要注意的是有些语言存在常量池，也就是并未拷贝值副本。<br />在 PHP 语言中，除了对象，其他变量都是传值。<br />在 Java 中，8 种基本数据类型都是传值。 |
| 引用   | 在 PHP 中存在引用，引用可以认为是指针，但是却又不能进行指针运算。<br />同一个引用的变量，一改全改。 |
| 地址值 | 在 Java 中出现地址，其实也是传值，不过传的指针的地址值。<br />同一个地址值的变量，存在重新赋值时，不会像 PHP 那样一改全改。 |

==注意==：地址 = 指针，地址值 ≠ 地址，地址值 = 指针的地址。

### 传值

传值很简单，就是将基本数据类型的值拷贝一份副本。

### 引用

下面的代码都是在测试框架下，验证，TDL（Test Drive Learn）

```php
$var1 = 'no';
// 引用赋值
$var2 = &$var1;
$var2 = 'yes';
$var3 = &$var2;
self::assertSame('yes', $var2);
self::assertSame('yes', $var1);
// 将变量名与引用解绑
unset($var2);
self::assertSame('yes', $var1);
self::assertSame('yes', $var3);
// 将引用对应的数据清空，一改全改，改了 $val3，$val1 也被改了，因为相同引用
$var3 = null;
self::assertNull($var3);
self::assertNull($var1);
```

PHP 手册

![image-20220819180019574](./images/image-20220819180019574.png)

引用定位

![image-20220819180323709](./images/image-20220819180323709.png)

### 地址值

Java 中的地址值传递。

```java
Integer a = 1;
Integer b = a;
// 这里只是将 b 与 1 解绑，与 2 地址值绑定
b = 2;
Assert.assertTrue(b.equals(2));
Assert.assertTrue(a.equals(1));
// 将 b 与 2 的地址值解绑
b = null;
Assert.assertNull(b);
Assert.assertTrue(a.equals(1));
```

```java
Integer c = 1;
Integer d = c;
// 这里只是将 c 与 1 解绑，与 2 地址值绑定
// c 与 1 的地址值解绑，并不影响 d
c = 2;
Assert.assertTrue(c.equals(2));
Assert.assertTrue(d.equals(1));
// 将 c 与 2 的地址值解绑
c = null;
Assert.assertNull(c);
Assert.assertTrue(d.equals(1));
```

## 总结

通过上面代码我们可以得出结论，PHP 中的对象才是真正的引用传递。而 Java 中的非基本数据类型传递的不是引用，而是引用的地址值，或者说是引用的地址值的副本。

PHP 中将`$var3`置`null`，导致`$val2`和`$val1`都为`null`。因为他们是引用，是地址，是指针。

Java 中`b`，`c`置`null`，而`a`和`d`的值却没有变。因为他们都是地址值，是指针的地址。

