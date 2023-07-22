# 2022年8月28日


上周给同事讲 Java 基础，有 2 个地方是错误的，特此纠正。

## 地址值赋值

我给人讲的是：

```java
Integer a = 1;
Integer b = a;
b = 2;
assert a == 2;// 错了，其实是 1
b = null;
assert b == null;
assert a = 2;// 错了，其实是 1
```

上面其实给人讲错了，变量`a`至始至终都是`1`，才是对的。

## Object.hashCode()的用处

> 我给人讲，hashCode() 主要用来加速 equals() 方法的。认为两个对象的 hashcode 相同，其一定相等。

其实讲错了，仔细思考一下，==相同的对象，其 hashcode 一定相同，但是 hashcode 相同的对象，就未必是一个对象了，存在 hash 碰撞。==就能推翻上面的言论。

再者，hashCode() 是用来给 equals 服务也是错的，其主要用来服务集合的，例如 HashMap。因为其内部的 get() 和 put() 都依赖了对象的 hashCode() 方法。
