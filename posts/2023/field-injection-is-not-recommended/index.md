# field injection is not recommended


idea 报黄线，也不是报错，就是膈应你，告诉你这种做法不推荐。

```java
import javax.servlet.http.HttpServletRequest;

public class HttpMountainAop {
	@Autowired
    private HttpServletRequest httpServletRequest;
}
```

![image-20230317085532212](/images/image-20230317085532212.png)

> 大概意思是：这种字段注入的方式是不被推荐的。

既然不被推荐那推荐什么呢？推荐使用构造方法注入。

```java
import javax.servlet.http.HttpServletRequest;

public class HttpMountainAop {
    private final HttpServletRequest httpServletRequest;

    public HttpMountainAop(HttpServletRequest httpServletRequest) {
        this.httpServletRequest = httpServletRequest;
    }
}
```


