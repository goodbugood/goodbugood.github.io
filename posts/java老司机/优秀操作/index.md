# Java推荐操作


# Java推荐操作

## 判空

### 不推荐用法

不推荐的原因是，不够语义化。

```java
@Test
public void npeMap() {
    HashMap<String, HashMap<String, String>> map = new HashMap<>(2);
    HashMap<String, String> hashMap = new HashMap<>(1);
    hashMap.put("country", "china");
    map.put("shali", hashMap);
    if (null != map.get("shali")) {
        HashMap<String, String> shali = map.get("shali");
        if (null != shali.get("country")) {
            Assertions.assertEquals("china", shali.get("country"));
        }
    }
}
```

### 推荐用法

```java
@Test
public void npeMap() {
    HashMap<String, HashMap<String, String>> map = new HashMap<>(2);
    HashMap<String, String> hashMap = new HashMap<>(1);
    hashMap.put("country", "china");
    map.put("shali", hashMap);
    if (Objects.nonNull(map.get("shali"))) {
        HashMap<String, String> shali = map.get("shali");
        if (Objects.nonNull(shali.get("country"))) {
            Assertions.assertEquals("china", shali.get("country"));
        }
    }
}
```

## 链式调用空指针检查Optional

### 错误用法

链式调用，存在 `NullPointerException` 的危险。

```java
@Test(expected = NullPointerException.class)
public void npeMap() {
    HashMap<String, HashMap<String, String>> map = new HashMap<>(2);
    HashMap<String, String> hashMap = new HashMap<>(1);
    hashMap.put("country", "china");
    map.put("shali", hashMap);
    Assertions.assertEquals("china", map.get("shali").get("country"));
    // 注解断言抛出空指针异常 NullPointerException
    Assertions.assertEquals("china", map.get("tony").get("country"));
}
```

### 正确用法

```java
@Test
public void npeMap() {
    HashMap<String, HashMap<String, String>> map = new HashMap<>(2);
    HashMap<String, String> hashMap = new HashMap<>(1);
    hashMap.put("country", "china");
    map.put("shali", hashMap);
    Assertions.assertEquals("china", Optional.ofNullable(map).map(mapper -> mapper.get("shali")).map(mapper -> mapper.get("country")).orElse(null));
    // 此处不会抛 NullPointerException
    Assertions.assertNull(Optional.ofNullable(map).map(mapper -> mapper.get("tony")).map(mapper -> mapper.get("country")).orElse(null));
}
```

### 其他语言的链式判空操作

#### PHP8.0

PHP 8.0 引入了1个新特性，指针安全运算符。

```php
string name = shali()?->info()?->name();
```

`?->` 就是指针安全运算符 `null safe`。

#### Kotlin

kotlin 定义了 2 个语法糖，`?.` 不为 null 时的处理，`?:` 为 null 时的处理。

`?:` 这个 PHP 使用者很熟悉，就是为空时的处理语法糖。

```kotlin
// 不为 null 时的操作
val files = File("Test").listFiles()
println(files?.size)

// 不为 null 时的操作和为 null 时的操作
val files = File("Test").listFiles()
println(files?.size ?: "empty")
```

#### Swift

跟 kotlin 很像，但是他还支持数组的元素的判断。

## 返回值为集合的方法正确返回方式

### 返回值List

```java
public List<String> getNumbers() {
    // 不建议
    // return new ArrayList<>();
    return Collections.emptyList();
}
```

### 返回Map

```java
public Map<Object, Object> getNumbers() {
    // 不建议
    // return new HashMap<>();
    return Collections.emptyMap();
}
```

### 返回Set

```java
public Set<String> getNumbers() {
    // 不建议
    // return new HashSet<>();
    return Collections.emptySet();
}
```


