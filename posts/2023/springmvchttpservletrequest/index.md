# SpringMVC 获取请求参数的 N 种方式


## 控制器请求方法里注入 HttpServletRequest

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpServletRequest;

/**
 * @author Shali
 */
@RestController
@RequestMapping("test")
public class TestController {
    @GetMapping("hello")
    public String hello(HttpServletRequest httpServletRequest) {
        return httpServletRequest.getQueryString();
    }
}
```

## 利用 Spring 的依赖注入到控制器属性中

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpServletRequest;

/**
 * @author Shali
 */
@RestController
@RequestMapping("test")
public class TestController {
    @Autowired
    private HttpServletRequest httpServletRequest;

    @GetMapping("hello")
    public String hello() {
        return httpServletRequest.getQueryString();
    }
}
```

## 利用请求上下文获取 HttpServletRequest

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.servlet.http.HttpServletRequest;
import java.util.Objects;

/**
 * @author Shali
 */
@RestController
@RequestMapping("test")
public class TestController {
    @GetMapping("hello")
    public String hello() {
        HttpServletRequest httpServletRequest = ((ServletRequestAttributes) Objects.requireNonNull(RequestContextHolder.getRequestAttributes())).getRequest();
        return httpServletRequest.getQueryString();
    }
}
```


