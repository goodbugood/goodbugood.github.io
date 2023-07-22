# 统一错误码如何追加具体业务错误描述


今天阅读了他人代码，作者应该是想封装一个统一的错误码枚举类。这种需求大家应该都遇到过。

## 常见封装

```java
package com.baidu;

import lombok.AllArgsConstructor;
import lombok.Getter;

@AllArgsConstructor
public enum ErrorCode {
    OK("0", "ok"),
    FAIL("1", "failed");

    @Getter
    private final String code;
    @Getter
    private final String msg;
}
```

使用：

```java
ErrorCode.FAIL.getMsg();
```

这种是最基本的错误统一码，但是不足以描述具体的错误异常。

## 同事改造版

鉴于上面的统一错误异常同事对他进行了改造。

```java
package com.baidu;

import lombok.AllArgsConstructor;
import lombok.Getter;

@AllArgsConstructor
public enum ErrorCode {
    OK("0", "ok"),
    FAIL("1", "failed：%s");

    @Getter
    private final String code;
    @Getter
    private final String msg;
}
```

使用：

```java
String.format(ErrorCode.FAIL.getMsg(), "具体的业务错误描述");
```

## 优化版

看了他的代码，我觉得写的不够优雅，虽然功能实现了。

写了大量的 `String.format`。

```java
package com.baidu;

import lombok.Getter;

public enum ErrorCode {
    OK("0", "ok"),
    FAIL("1", "failed");

    @Getter
    private final String code;
    @Getter
    private final StringBuilder msg;

    ErrorCode(String code, String msg) {
        this.code = code;
        this.msg = new StringBuilder(msg);
    }
}
```

使用：

```java
ErrorCode.FAIL.getMsg().append("：具体的业务错误描述");
```

## 对比

```java
package com.baidu;

import lombok.AllArgsConstructor;
import lombok.Getter;

import java.util.Objects;

@AllArgsConstructor
enum ErrorCode1 {
    OK("0", "ok"),
    FAIL_PARAM("110", "参数缺失：%s");
    @Getter
    private final String code;
    @Getter
    private final String msg;
}

enum ErrorCode2 {
    OK("0", "ok"),
    FAIL_PARAM("110", "参数缺失");

    ErrorCode2(String code, String msg) {
        this.code = code;
        this.msg = new StringBuilder(msg);
    }

    @Getter
    private final String code;
    @Getter
    private final StringBuilder msg;
}

public class ErrorCode {
    public static void main(String[] args) {
        String name = null;
        Integer age = null;
        // msg 使用占位符
        if (Objects.isNull(name)) {
            String msg = String.format(ErrorCode1.FAIL_PARAM.getMsg(), "姓名不能为空");
            System.out.println(msg);
        }
        if (Objects.isNull(age)) {
            String msg = String.format(ErrorCode1.FAIL_PARAM.getMsg(), "年龄不能为空");
            System.out.println(msg);
        }
        // msg 使用 SB
        if (Objects.isNull(name)) {
            String msg = ErrorCode2.FAIL_PARAM.getMsg().append("：姓名不能为空").toString();
            System.out.println(msg);
        }
        if (Objects.isNull(age)) {
            String msg = ErrorCode2.FAIL_PARAM.getMsg().append("：年龄不能为空").toString();
            System.out.println(msg);
        }
    }
}
```

### 输出结果有点问题

```java
参数缺失：姓名不能为空
参数缺失：年龄不能为空
参数缺失：姓名不能为空
参数缺失：姓名不能为空：年龄不能为空
```

这个粘连错误消息的问题，可能需要解决。

