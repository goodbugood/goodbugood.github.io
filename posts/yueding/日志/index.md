# 日志


> 约定大于配置，配置大于编码。
>
> 日志是系统运行的“照妖镜”，通过它能够实时反映系统的运行状态。

## 日志的打印也需要设计

如果日志的打印不设计，就会出现以下问题：

- 开发者各自为政，日志怎么打全凭个人喜好，无统一的规范
- 打了一些毫无意义的日志，徒增日志文件大小
- 阅读日志的人不知日志中打印参数的含义，还要结合日志打印的位置→业务代码→参数位置→参数注释，来学习了解

## When

什么时候打日志，当你需要日志才能定位生产环境的问题，而不是看代码猜测的时候，就是你需要打日志的时候。

### HTTP/RPC接口调用时

接口调用无论是调用成功还是调用失败，均要打印请求和成功/失败响应日志。

### 程序抛出Exception时，不向上抛就打印异常堆栈

注意，不能记录异常日志后，继续向上抛异常。

但是接口调用除外，因为我们可能需要打印错误的响应报文后，再往上抛接口异常。

### 不太可能进入的条件分支，要打印条件值

例如年龄小于 0 的分支。

### 关键执行步骤和中间态

例如，优惠的计算和分发，每次达到的条件，和计算出的优惠。都要打日志，便于后续跟踪优惠的计算。

### 函数的入口和出口处

## Where

什么地方打日志

- 容易发生扯皮地方

例如，调别人接口的地方，一定要`MUST`打日志，哪怕你现在对接，运行正常，谁能保证永远不出问题，一旦到时候出问题了，谁有日志，谁就有主导权，以日志服人。

看到有的文章说，接口日志对接调试的时候打，上线了就不需要打了。你能保证你对接完上线后，不改代码，你能保证别人不改吗？如果哪天报错了，你说别人的问题，别人让你提供现场日志，你说你没有？多么无力。

- 程序加载的地方

毕竟加载的程序，只会走一次，只需要打一次，就能方便后面定位错误，不信你看 SpringBoot 启动时就打了 INFO 类型的启动日志。

这里加载一次，指的是常驻内存的应用。

### 对接外部的接口的封装，必须打上下行日志，防扯皮

### 程序重要状态信息变化要记录，方便还原现场

### 系统出入口，重要方法出入口

记录输入和输出，方便定位。

### 任何发生异常的地方

任何异常都应该被记录。

### 很少可能走到的 else 分支

此 else 分支要记录。

### 耗时的任务执行要记录进度

## What

- 打文件名和行号

为啥要打文件名和行号？通过错误描述不能定位吗？

有时候我们的项目中会有很多的 copy 代码，甚至错误日志都 copy，别人 copy 了你的错误日志描述，到时候打出来了，算你的，没有文件名和行号，你要跟他扯皮吗。

- 打出问题的时间，最好精确到毫秒

并发性的问题，用秒做单位的时间，有时候真不好排错。

- 异常问题，打印异常堆栈
- 打上下文内容，如果不打上下文，就相当于报警说这家超市里有小偷，打了上下文就相当于报警说工号 007 的那位员工是小偷。上下文内容能帮你快速，准确地定位错误。

## How

怎样打日志？

### 不使用具体的日志框架而是使用抽象的日志框架

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

private static final Logger logger = LoggerFactory.getLogger(Xxx.class);
```

即不使用`logback`，而使用`slf4j`。

### 系统程序错误是Error级别，用户参数错误是Warning级别

高效地打日志

- 对于 debug 日志级别日志，必须判断 debug 级别后才能打印日志
- 使用格式化的占位符打日志，而不是字符串拼接日志，占位符的效率远远高于拼接字符串
- 异常时不能打印完日志，再向上抛异常，向上的异常捕获者又打印了异常日志。应该交给异常的最终捕获处理者来打日志。
- 不要在循环中打印日志，最好的办法是在循环中统计单个日志，循环外打印统计的日志。跟不要在循环中执行 sql 一个道理。
- 禁止线上环境开启 debug 日志级别，因为除了你写了少部分的 debug 级别日志，可以你用的依赖库，框架可能写了大量的 debug 日志，这样会塞爆你的服务器的。
- 不要使用错误的日志级别，你在 info 日志文件中的打的 error 日志，同事在 error 日志文件中死活找不到。
- 不要使用 `e.printStackTrace()`，因为他的实现是打印日志到控制台，日志文件里是不会记录的

### 日志级别

![img](/images/1620.png)

### 日志格式

- 参数使用 {} 占位符，而不是字符串拼接，因为这样只有日志真正打印的时候，才会处理参数，降低了日志系统的负载
- 使用 [] 分隔参数，提高日志的可读性
- 每一个请求最好有一个 requestId，记入日志，同时记录线程名称

## 疑问

说打印日志时，不要直接 json 化对象，那么 LOG.error("xxx", exception) 中又传递对象，需要确认 error 的实现。

## 优雅的日志处理需求

- 能自动将异常错误,转换成对前端友好的错误输出，并记录调用栈和出错
- 能准确定位前端的错误提示对应的异常错误日志，以及请求参数
