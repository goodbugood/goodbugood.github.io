# 送你100个异常


## java.lang

### java.lang.IllegalStateException

状态异常。

## Spring

### org.springframework.beans.factory.NoSuchBeanDefinitionException

当你从 Spring 容器（）中获取 bean 时，如果这个 bean 不存在，他并不会返回 null，而是直接抛未定义 bean 异常。

出现这种问题一般 2 种：

1. Spring 容器里就没有这个 bean
2. 或者获取方式不对，bean 按名称注入，但是按类型获取，也会造成不存在。

### org.springframework.beans.BeanInstantiationException

Spring Bean 初始化异常。

### org.springframework.beans.factory.UnsatisfiedDependencyException

```sh
org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'com.shali.ruby.test.AutowiredTest': Unsatisfied dependency expressed through field 'people'; nested exception is org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type 'com.shali.ruby.bean.People' available: expected single matching bean but found 2: maria,tony
```

大概意思是说根据你要注入的 `com.shali.ruby.bean.People` bean，但是按类型注入，发现了 2 个此类的 bean，他不知道该注入哪个实现 bean。

### org.springframework.boot.loader.thin.ThinJarLauncher

```sh
Exception in thread "main" java.lang.ClassNotFoundException: org.springframework.boot.loader.thin.ThinJarLauncher
```

很多同学学习了 SpringBoot 减肥打包法，屡试不爽。

出现这个原因是，仓库里找不到`spring-boot-thin-launcher-1.0.28.RELEASE-exec.jar`文件，于是跑到仓库`C:\Users\用户名\.m2\repository\org\springframework\boot\experimental\spring-boot-thin-launcher\1.0.28.RELEASE`目录下，发现存在阿。其实是你网络的不好，下载的文件不全，大小都不对。

建议删除`spring-boot-thin-launcher-1.0.28.RELEASE-exec.jar`，重新跑下`java -jar 项目.jar`命令，他会检查并安装依赖的。

## java.lang.IllegalMonitorStateException

字面意思：非法监控状态异常

出现这种问题，多是你在非同步代码中调用了`Object.wait()`或`Object.notify()`或`Object.notifyAll()`方法。

正确调用上述方法的姿势是，首先拿到锁。

## java.lang.IndexOutOfBoundsException

数组越界异常

## MyBatis

### org.apache.ibatis.binding.BindingException

org.apache.ibatis.binding.BindingException: Invalid bound statement (not found):

无效绑定异常，即提示你，XxxMapper 接口与 XxxMapper.xml 文件没有绑定成功。

## 常见提示

### Raw use of parameterized class 'FutureTask'

翻译过来就是，参数类 FutureTask 的原始使用，就是说你使用泛型类 FutrueTask 却不指定泛型，提示你存在数据类型丢失的风险，我们知道指定泛型就是避免数据类型丢失的风险。
