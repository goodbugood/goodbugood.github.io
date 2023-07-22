# 如何获得方法/函数调用处的文件名和行号


如何获取方法调用处的文件名，行号？

## 需求

很多时候我们打印日志的时候，需要获取日志被打印的位置，具体来说就是文件名和行号。或者类的完整限定名和行号也行。只要能让我们精准定位打印的位置即可。

## 实现

在 PHP 中我们知道有 2 个魔术常量，真的很好用，`__FILE__`和`__LINE__`。很多时候我们使用他获取当前的文件名和行号。

### 错误实现

```php
class Test {
    /**
     * @param $message
     */
    private function log($message)
    {
        echo sprintf('File: %s Line: %d Info: %s', __FILE__, __LINE__, $message);
    }

    public function testLog()
    {
        $this->log('进入业务方法中了');
    }
}
```

打印的行号不是调用处的行号，而是 \_\_LINE\_\_ 所处的行号。

曲线救国：

```php
class Test {
    /**
     * @param $message
     */
    private function log($file, $line, $message)
    {
        echo sprintf('File: %s Line: %d Info: %s', $file, $line, $message);
    }

    public function testLog()
    {
        $this->log(__FILE__, __LINE__, '进入业务方法中了');
    }
}
```

上面的实现确实能够精准定位调用处的文件和行号，但是方法的调用不好用。每次都需要带上形参文件名和行号。重复啰嗦。

能不能优化？

### PHP

PHP 获取调用堆栈位置。

我们知道代码的执行是先压栈，然后再弹栈，依次执行调用关系。那我们完全可以借助调用栈里的信息来解决上面问题。

```php
class Test {
    /**
     * @param $message
     */
    private function log($message)
    {
        $debug_backtrace = debug_backtrace(DEBUG_BACKTRACE_IGNORE_ARGS, 1);
        echo sprintf('File: %s Line: %d Info: %s', $debug_backtrace[0]['file'], $debug_backtrace[0]['line'], $message);
    }

    public function testLog()
    {
        $this->log('进入业务方法中了');
    }
}
```

改良后，打印的行号和文件号就不再是定义处而是调用处，符合要去。

关于 `debug_backtrace()` 函数的参数使用，`DEBUG_BACKTRACE_IGNORE_ARGS` 获取调用栈数组，不包含参数，这样能够节省运行时内存，`1` 表示仅返回调用栈的栈顶元素，也就是仅返回 1 条，节省运行时内存。如果是 0 代表返回整个调用栈。

### Java

```java
package com.shali.ruby.bean;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Test {
    private static final Logger log = LoggerFactory.getLogger(Test.class);
    
    public void testLog()
    {
        this.log.info('进入业务方法中了');
    }
}
```

打印日志：

```markdown
行号 27 2022-09-14 21:09:00.743 DEBUG 22356 --- [http-nio-80-exec-2] com.shali.ruby.bean.Test : 进入业务方法中了
```

上面日志打印了日志打印调用处的行号，时间，进程号，线程号，包名，日志信息。

## 原理


