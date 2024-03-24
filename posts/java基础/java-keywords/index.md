# Java关键字


## final

### 修饰变量

`final` 修饰的变量，只能初始化，不能赋值。（即只能被赋值 1 次。）

```java
// 定义并初始化
final Integer integer = 1;
// 第二次和第三次赋值都会报错
// Cannot assign a value to final variable 'integer'
integer = 2;
integer = 3;
```

```java
// 只定义未初始化
final Integer integer;
// 正确
integer = 2;
// 报错
// Cannot assign a value to final variable 'integer'
integer = 3;
```

### 修饰方法

1. 被 final 修饰的方法，可以被重载（Overload），但是不能被重写（Override）
2. 被 final 修饰的静态方法，也遵循上述规则，子类可以重载，但不能重写

### 修饰类


