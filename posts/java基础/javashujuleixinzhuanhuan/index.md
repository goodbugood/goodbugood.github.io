# Java数据类型转换


## 基本数据类型和包装数据类型互转

### 基本数据类型转包装数据类型

```java
int int1 = 1;
// 基本数据类型转包装数据类型：显式转换
Integer integer = Integer.valueOf(1);
// 隐式转换
Integer integer1 = int1;
assertEquals(integer, integer1);
char a = 'a';
// 显式转换
Character character = new Character(a);
// 隐式转换
Character character1 = a;
assertEquals(character, character1);
// 布尔
boolean b = false;
Boolean b2 = new Boolean(b);
Boolean b3 = b;
assertEquals(b2, b3);
```

### 包装数据类型转基本数据类型

```java
// 包装数据类型转基本数据类型
Integer integer1 = new Integer(123);
Integer integer2 = 123;
assertEquals(integer1, integer2);
int i1 = integer1.intValue();
int i2 = integer2;
assertTrue(i1 == i2);
// 字符类型
Character character = new Character('b');
Character character1 = 'b';
assertEquals(character, character1);
char c = character.charValue();
char c1 = character1.charValue();
assertEquals(c, c1);
```

## 基本数据类型数组和包装数据类型数组互转

### 第三方工具库

```java
import cn.hutool.core.util.ArrayUtil;

int[] ids = {1, 2, 4};
// 基本类型数组转包装类型数组
Integer[] integers = ArrayUtil.wrap(ids);
// 包装数据类型数组转基本数据类型数组
int[] ints1 = ArrayUtil.unWrap(integers);
```

原理看源码。

### stream流处理

```java
int[] ids = {1, 2, 4};
// 基本类型数组转包装类型数组
Integer[] integers = (Integer[]) Arrays.stream(ids).boxed().toArray();
```

报错了：

> java.lang.ClassCastException: [Ljava.lang.Object; cannot be cast to [Ljava.lang.Integer;

原因：

>  虽然 Integer 是 Object 的子类，但是 `Integer[]` 不是 `Object[]` 的子类，因为 `Integer[]` 是数组，我们都知道数组的父元素是 Object，可不是 `Object[]`。

所以不能有如下操作：

```java
Object[] objects;
Integer[] integers = (Integer[]) objects;
```

## 数组转集合

### 包装类型数组转集合

```java
// 包装数据类型转集合
Integer[] integers = new Integer[]{1, 2};
List<Integer> integerList = Arrays.stream(integers).collect(Collectors.toList());
assertEquals(2, integerList.size());
assertEquals(1, (int) integerList.get(0));
assertEquals(2, (int) integerList.get(1));
```

### 基本数据类型数组转集合

注意我们知道集合只能存放对象，不能存放基本数据类型，所以要将基本数据类型先转成包装数据类型。

```java
// 基本数据类型数组转集合
int[] ints = new int[]{1, 2, 3};
List<Integer> integerList1 = Arrays.stream(ints).boxed().collect(Collectors.toList());
assertEquals(3, integerList1.size());
assertEquals(1, (int) integerList1.get(0));
assertEquals(3, (int) integerList1.get(2));

// 或者
String array[]= {"hello", "world"};
List<String> list = new ArrayList<String>(Arrays.asList(array));

// 这种操作，返回的一个静态内部类，少了一些方法，例如 add，remove
java.util.Arrays.ArrayList<String> list = Arrays.asList(array);
```
