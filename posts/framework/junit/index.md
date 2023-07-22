# JUnit


## 疑问

### assertSame与assertEquals的区别

```java
/**
 * 公
 */
class Gong {
    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }

        return "男".equals(obj.toString());
    }
}

@Test
public void sameAndEquals() {
    Gong gong1 = new Gong();
    Gong gong2 = new Gong();
    String nan1 = "男";
    String nan2 = "男";
    StringBuilder nan3 = new StringBuilder("男");
    // 比较的是地址值
    Assert.assertNotSame(gong1, nan1);
    Assert.assertNotSame(gong1, gong2);
    // 字符串常量池，导致 nan1 与 nan2 的地址值相同
    Assert.assertSame(nan1, nan2);
    // 地址值不一样
    Assert.assertNotSame(nan1, nan3);
    // assertEquals 比较的是 gong1.equals() 方法的实现
    Assert.assertEquals(gong1, nan1);
    // 地址值不同，由于未重写 toString() 所以也不同
    Assert.assertNotEquals(gong1, gong2);
    // 地址值相同
    Assert.assertEquals(nan1, nan2);
    // equals 重写，地址值不同，但是 toString() 的内容相同
    Assert.assertEquals(gong1, nan3);
}
```

==总结==：

- assertSame 比较的是内存地址

- assertEquals 比较的是类重写的 Object.equals() 方法，如果类未重写，则与 assertSame 功能相同
