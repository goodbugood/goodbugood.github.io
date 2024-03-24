# 第一章


## javascript 三大类变量类型

**3种基本数据类型**

> 字符串(String) 数字(Number) 布尔(Boolean)

**2种复合数据类型**

> 对象(Object) 数组(Array)

**2种特殊数据类型**

> Null undefined

1. 除了 null undefined 没有 toString() 继承方法，其他变量类型都有。

2. 对象的继承属性用 `delete` 删除表达式无法删除

## 引用传值

> 请注意在 js 中，数组和对象是引用传值的.那么问题来了,在赋值的时候出现了新的问题了,改变 A 值的同时也会改变 B 值.

解决办法:

深度复制法:

```javascript
var cloneObj = JSON.parse(JSON.stringify(obj));
```

上面这种方法好处是非常简单易用，但是坏处也显而易见，这会抛弃对象的constructor，也就是深复制之后，无论这个对象原本的构造函数是什么，在深复制之后都会变成Object。另外诸如RegExp对象是无法通过这种方式深复制的。
