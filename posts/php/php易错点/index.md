# PHP易错点


# PHP 易错点

## PHP 内置回调事件

```php
# 程序结束（包含正常结束和异常结束）时，触发此回调
register_shutdown_function(function () {
    // 获取上一个错误，如果是正常结束返回的是 NULL
    $error = error_get_last();
});
# 注册 \Exception 抛出时的回调处理
set_exception_handler(function (\Exception $exception) {
});
# 注册 \Error 抛出时的回调处理
set_error_handler(function ($iErrNo, $sErrStr, $sErrFile, $iErrLine) {
});
# 注册类加载
spl_autoload_register(function ($sClassName) {
});
```

### 注册完，支持运行时移除的回调

#### restore_error_handler

```php
function a() {}
function b() {}
// 注册错误处理函数 a
set_error_handler('a');
// 修改错误处理函数为 b
set_error_handler('b');
// 恢复为原来错误处理函数 a
restore_error_handler();
```

#### restore_exception_handler

```php
<?php
    function exception_handler_1(Exception $e)
    {
        echo '[' . __FUNCTION__ . '] ' . $e->getMessage();
    }
    function exception_handler_2(Exception $e)
    {
        echo '[' . __FUNCTION__ . '] ' . $e->getMessage();
    }

    set_exception_handler('exception_handler_1');
    set_exception_handler('exception_handler_2');

    restore_exception_handler();
    throw new Exception('This triggers the first exception handler...');
	// 输出结果：[exception_handler_1] This triggers the first exception handler...
?>
```

## 高手擅长回避的问题

### 非 0 即真

```php
self::assertSame(7, 7 ?: 1);
self::assertSame(1, 0 ?: 1);
// 你是不是以为下面的结果是 1 啊
self::assertSame(-2, -2 ?: 1);
```

总结：

```php
$var3 = (非 0 即真) ? $va1 : $val2;
```

知道了非 0 即真，那么下面的答案就简单了。

```php
if (-1) {
    var_dump(1);
} else {
    var_dump(0);
}
```

输出：int(1)

### PHP 8.0 终于对了

## OPcache 和 OPcode 的关系

最近你是不是也被这个问题困惑，那我们一起来翻阅 PHP 手册来需求解答。
在 PHP 手册的 `函数参考 > 影响 PHP 行为的扩展 > OPcache` 有专业的解答。

> OPcache 通过将 PHP 脚本预编译的字节码存储到共享内存中来提升 PHP 的性能， 存储预编译字节码的好处就是 省去了每次加载和解析 PHP 脚本的开销。

那么 OPcode 是什么？你还没解释。

什么，不是说了吗？ OPcode 就是字节码，相当于 Java 开发中的 xxx.class，PHP 的 Zend VM 解析引擎就类似 Java 的 VM

说白了就是，用 OPcache 这个扩展来缓存 OPcode 到内存，从而来提高 PHP 的解析速度。

更多使用技巧请参考: [使用 OpCache 提升 PHP 性能](https://segmentfault.com/a/1190000002523558)

## 内存中的堆栈区别

很多是否还是会碰到堆栈，那堆栈是什么，区别又是什么，是时候花时间弄明白了。

| 栈   | 堆   |
| ---- | ---- |
|大小|不大，最大也就10M|可以分配大量内存空间|
|手动释放内存|不需要，临时空间<br>函数退出后系统释放|手动释放|
|存放数据|方法调用，所有局部变量和函数的参数|操作系统分配，手动释放，最终 OS 释放|

## const 与 define 有何区别

我们知道 const 和 define 都可以定义全局常量，那么他们有和区别呢？

```php
<?php

const VERSION = '3.2.3';

defined('ADDON_PATH') or define('ADDON_PATH', APP_PATH . 'Addon');
```

总结一下，就是，如果定义的常量没有其他常量条件限制，就使用`const`常量定义语法，如果有条件限制，则使用宏定义`define`来定义常量。

## var_export 高级用法

高级应用，代码压缩。

## 字符串方法使用

### strpos 字符串匹配

- 不推荐的用法

```php
<?php
    
if (!strpos('hello', 'h')) {
    exit('不存在 h');
} else {
    exit('存在 h');
}
```

结果：不存在 h

原因：`strpos('hello', 'h')` 返回的索引是 0，由于弱类型，被当作 false 处理了。

- **推荐用法**

```php
<?php
    
if (false === strpos('hello', 'h')) {
    exit('不存在 h');
} else {
    exit('存在 h');
}
```

结果：存在 h
