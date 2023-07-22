# Maven


## 继承

- 子项目不继承父项目的 modules

## properties

全局属性，只对当前工程生效。

## dependencys

```xml
<dependency>
    <groupId>公司名称/公司域名</groupId>
    <artifactId>项目/模块名称</artifactId>
    <version>x.y.z格式版本号</version>
    <scope>生效环境</scope>
</dependency>
```

生效环境：

![image-20220902104025274](./images/image-20220902104025274.png)

## 常见错误

### No valid Maven installation found

> Error running 'bbs [clean]': No valid Maven installation found. Either set the home directory in the configuration dialog or set the M2_HOME environment variable on your system.

从描述看，就是因为找不到`mvn`工具的路径，导致无法使用`maven`解决依赖。

![image-20220826085757764](./images/image-20220826085757764.png)

## 疑问

- maven是如何解决循环依赖的？

## 引用

- [廖雪峰《Maven基础》](https://www.liaoxuefeng.com/wiki/1252599548343744/1309301178105890)
