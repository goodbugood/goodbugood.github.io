# idea 控制台中文乱码


## 现象

idea 的 Spring Boot 项目，使用了外置的 tomcat web 容器，跑起来就乱码了，网络上找了很多方案都未能解决。

```
24-Mar-2023 09:00:44.032 淇℃伅 [main] org.apache.catalina.startup.VersionLoggerListener.log Server.鏈嶅姟鍣ㄧ増鏈�: Apache Tomcat/8.5.64
```

## 解决办法

编辑虚拟机配置

![image-20230324085902830](./images/image-20230324085902830.png)

添加如下配置

```
-Dfile.encoding=UTF-8
```

![image-20230324090516479](./images/image-20230324090516479.png)

保存并退出，重启 idea 即可。

## 参考

- https://blog.csdn.net/qq_18335837/article/details/107481963
- https://www.cnblogs.com/ibigboy/p/11519412.html

