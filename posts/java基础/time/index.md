# Java日期时间


# Java日期时间

## JDK1.8之前

格式化时间：

```java
import cn.hutool.core.date.DatePattern;

import java.text.SimpleDateFormat;
import java.time.Instant;
import java.util.Date;

Instant instant = Instant.now();
Date date = Date.from(instant);
SimpleDateFormat simpleDateFormat = new SimpleDateFormat(DatePattern.NORM_DATETIME_PATTERN);
System.out.println(simpleDateFormat.format(date));
// 2023-05-05 22:02:582023-05-05 22:02:58
```

## JDK1.8之后

格式化时间：

```java
import cn.hutool.core.date.DatePattern;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern(DatePattern.NORM_DATETIME_PATTERN);
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println(localDateTime.format(dateTimeFormatter));
// 2023-05-05 22:36:27
```

## 时间戳

```java
@Test
void test() {
    // 获取时区
    String zoneId = ZoneId.systemDefault().getId();
    DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
    assertEquals("Asia/Shanghai", zoneId);

    // 时间戳转 java.time.LocalDateTime
    ZonedDateTime zonedDateTime = Instant.now().atZone(ZoneId.systemDefault());
    LocalDateTime localDateTime1 = zonedDateTime.toLocalDateTime();

    // java.time.LocalDateTime 转 java.util.Date
    LocalDateTime localDateTime = LocalDateTimeUtil.now();
    assertEquals(localDateTime.format(dateTimeFormatter), localDateTime1.format(dateTimeFormatter));
    Date date = Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());

    // 获取时间戳
    long nanoSecond;
    long millSecond;
    long second;
    // 方式一：
    millSecond = System.currentTimeMillis();
    System.out.printf("毫秒 System.currentTimeMillis ：%s%n", millSecond);
    nanoSecond = System.nanoTime();
    System.out.printf("纳秒 System.nanoTime() ：%s%n", nanoSecond);
    // 方式二：
    Instant instant = Instant.now();
    millSecond = instant.toEpochMilli();
    System.out.printf("毫秒 instant.toEpochMilli ：%s%n", millSecond);
    // instant.getNano() 返回的是时间戳的纳秒部分，可不是纳秒
    assertNotEquals(nanoSecond, instant.getNano());
    // 方式三：
    millSecond = new Date().getTime();
    // second = millSecond / 1000;
    second = instant.getEpochSecond();
    System.out.printf("秒 instant.getEpochSecond() ：%s%n", second);
}
```

## 2种时间的格式化方式

### 格式化时间

```java
import java.text.SimpleDateFormat;
import java.util.Date;

String pattern = "yyyy-MM-dd";
SimpleDateFormat simpleDateFormat = new SimpleDateFormat(pattern);
// 格式化时间
System.out.println(simpleDateFormat.format(new Date()));
```

### 时间被格式化

```java
import java.time.format.DateTimeFormatter;
import java.time.LocalDateTime;

// 时间被格式化
String pattern = "yyyy-MM-dd";
System.out.println(LocalDateTime.now().format(DateTimeFormatter.ofPattern(pattern)));
```


