# List常见问题


## ArrayList.add报IndexOutOfBoundsException

```java
// 使用身份证号作为索引，方便直接用ID获取某个人的信息
List<User> userList = new ArrayList<>(100);
User user = new User();
user.setId(23);
user.setName("我是张三");
userList.add(user.getId(), user);
```

报错：

> java.lang.IndexOutOfBoundsException: Index: 23, Size: 0
>
> ​	at java.util.ArrayList.rangeCheckForAdd(ArrayList.java:667)

**明明指定了容量 100，访问 23 的位置还报数组越界。因为他检查的是数组的大小，而不是容量**

看了源码，我们得出结论，只要索引值超过数组大小，或者索引值小于0，都会报索引越界。

```java
if (index > size || index < 0)
	throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
```

### 参考

- https://blog.csdn.net/vandavidchou/article/details/104306445

