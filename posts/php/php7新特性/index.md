# PHP7新版本新特性


## PHP7.4

### null合并运算符与三目运算符的区别

看代码：

```php
$remark = '';
// A ?: B，empty(A) 是真，则返回 A，否则返回 B
self::assertSame('maria', $remark ?: 'maria');
// A ?? B，A 非 null 则返回 A，否则返回 B
self::assertSame('', $remark ?? 'maria');
```

其实 null 合并运算符的出现就是为了解决三目运算符的。

![image-20220820142446882](./images/image-20220820142446882.png)

#### isset与is_null的区别

- 互为相反

```php
$name1 = 'tony';
unset($name1);
// $name1 值为 null
self::assertFalse(isset($name1));
self::assertTrue(is_null($name1));
// $name2 未设置，不存在
self::assertFalse(isset($name2));
self::assertTrue(is_null($name2));
```

```php
$name1 = 'maria';
self::assertSame(is_null($name1) ? '' : $name1, isset($name1) ? $name1 : '');
self::assertSame(is_null($name1) ? '' : $name1, $name1 ?? '');
self::assertSame(isset($name1) ? $name1 : '', $name1 ?? '');
```

- isset能够同时检查多个变量

这些变量之前为且的关系，即全部为真则返回真。

