# Java中的数组参数与可变参数的区别


# Java中的数组参数与可变参数的区别

看代码：

```java
private int[] func1(int... args) {
    return args;
}

private int[] func2(int[] args) {
    return args;
}

@Test
void func() {
    int[] args = {1, 2, 3};
    assertArrayEquals(args, func1(args));
    assertArrayEquals(args, func2(args));
    assertArrayEquals(args, func1(1, 2, 3));
}
```

其实可变参数，最终进入函数内也是数组。
