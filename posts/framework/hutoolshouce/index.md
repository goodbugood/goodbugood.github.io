# hutool 框架使用笔记


## json

### JSONArray转集合

```java
String json = "[2, 3, 5]";
JSONArray jsonArray = JSONUtil.parseArray(json);
// 转包装类型数组
List<Integer> integerList = jsonArray.toList(Integer.class);
```

### JSONArray转包装类型数组Integer[]

- 方案一，想当然地强转，报错

```java
// Object[] objects = jsonArray.toArray();
Integer[] integers = (Integer[]) jsonArray.toArray();
```

> 报错：java.lang.ClassCastException: [Ljava.lang.Object; cannot be cast to [Ljava.lang.Integer;

正确的方法：

```java
Object[] objects = jsonArray.toArray();
Integer[] integers = Arrays.copyOf(objects, objects.length, Integer[].class);
```

- 方案二

```java

```

- 方案三：最笨的办法，使用逗号分割 json 串数组

```java
String json = "[2, 3, 5]";
int[] ints = {2, 3, 5};

// 去除 json 数组字符串两侧的 [，]
String substring = json.substring(1);
String substring1 = substring.substring(0, substring.length() - 1);
// 使用逗号分割字符串
String[] strings = substring1.split(", ");
int[] ints3 = new int[strings.length];
for (int i = 0; i < strings.length; i++) {
    ints3[i] = Integer.parseInt(strings[i]);
}
assertArrayEquals(ints, ints3);
```

### JSONArray转基本类型数组

- 方案一

```java
// JSONArray 转基本类型数组
String json = "[2, 3, 5]";
JSONArray jsonArray = JSONUtil.parseArray(json);
// 先转包装类型数组
List<Integer> integerList = jsonArray.toList(Integer.class);
// 然后集合转转数组
int[] ints1 = integerList.stream().mapToInt(Integer::intValue).toArray();
int[] ints = {2, 3, 5};
assertArrayEquals(ints, ints1);
```

- 方案二

```java
// 方法二：
Object[] objects = jsonArray.toArray();
int[] ints2 = new int[objects.length];
for (int i = 0; i < objects.length; i++) {
    ints2[i] = (int) objects[i];
}
assertArrayEquals(ints, ints2);
```

## 日期

### 常用时间格式枚举

#### 2023-03-31 18:51:00

```java
// 获取当前时间
String now = DateUtil.now();

//
DateTime date = DateUtil.date();
Date date1 = new Date();

// 获取当前时间对象
String now2 = date.toString();

// 格式化：public static final String NORM_DATETIME_PATTERN = "yyyy-MM-dd HH:mm:ss";
String now3 = DateUtil.format(date1, DatePattern.NORM_DATETIME_PATTERN);
String now4 = DateUtil.formatDateTime(date1);
```

#### 2023-03-31

```java
// public static final String NORM_DATE_PATTERN = "yyyy-MM-dd";
DateTime date = DateUtil.date();
Date date1 = new Date();
String day = DateUtil.format(date, DatePattern.NORM_DATE_FORMAT);
String day1 = DateUtil.format(date1, DatePattern.NORM_DATE_FORMAT);
String day2 = date.toString(DatePattern.NORM_DATE_PATTERN);
assertEquals(day, day1);
assertEquals(day1, day2);
```

#### 18:51:00

```java
// public static final String NORM_TIME_PATTERN = "HH:mm:ss";
DateTime date = DateUtil.date();
Date date1 = new Date();
String time = DateUtil.format(date, DatePattern.NORM_TIME_PATTERN);
String time1 = DateUtil.format(date1, DatePattern.NORM_TIME_PATTERN);
String time2 = date.toString(DatePattern.NORM_TIME_PATTERN);
assertEquals(time, time1);
assertEquals(time1, time2);
```

#### Fri Mar 31 19:09:39 CST 2023

```java
DateTime date = DateUtil.date();
Date date1 = new Date();
// Fri Mar 31 19:22:42 CST 2023
String now5 = date1.toString();
String now6 = date.toString(DatePattern.JDK_DATETIME_FORMAT);
assertEquals(now5, now6);
// 星期五 三月 31 19:22:42 CST 2023
String now7 = date.toString(DatePattern.JDK_DATETIME_PATTERN);
```


