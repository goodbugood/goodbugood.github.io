# Tomcat


## 问

### Tomcat 与 Catalina 的关系

![image-20230428151754121](/images/image-20230428151754121.png)

## 中文乱码

### tomcat框架打印的中文不乱码，业务代码输出的乱码

![image-20230506165507912](/images/image-20230506165507912.png)

只需要添加红框内的参数，即可解决自行输出的中文乱码问题。

## 日志文件位置

### idea外挂tomcat日志文件路径

```xml
06-May-2023 17:04:36.219 信息 [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE:     C:\Users\Shali\AppData\Local\JetBrains\IntelliJIdea2021.2\tomcat\d80e4829-06ff-4802-b054-c68a1428377f
06-May-2023 17:04:36.219 信息 [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_HOME:     C:\Users\Shali\Downloads\apache-tomcat-8.5.64
```

`C:\Users\Shali\AppData\Local\JetBrains\IntelliJIdea2021.2\tomcat\d80e4829-06ff-4802-b054-c68a1428377f\logs`

