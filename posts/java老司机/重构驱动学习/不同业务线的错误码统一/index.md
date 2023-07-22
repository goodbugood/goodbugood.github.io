# 不同业务线的错误码统一规划


## 分离

我们想实现如下效果：

1. 领导层定义，不同业务线的错误码，也是错误码的前几位
2. 不同业务线的开发，定义自己内部的错误码，也就是错误码后面的
3. 如果领导层调整不同业务线的错误码，业务线的同事不受影响（领导层文化喜欢改动这些，你懂的）

那么既然要达到如上效果，就需要我们去分离这两个码。

```java
package com.baidu;

import lombok.Getter;
import lombok.ToString;

/**
 * 老板负责定义大的方向，业务方向的错误码前缀
 */
interface CommonErrorCode {
    // 系统错误码
    public final String PREFIX_ERROR_CODE_SYSTEM = "100";
    // 会员错误码
    public final String PREFIX_ERROR_CODE_MEMBER = "200";
    // 商城错误码
    public final String PREFIX_ERROR_CODE_SHOP = "300";
}

/**
 * 会员领域负责定义本领域的错误码
 */
@ToString
enum MemberErrorCode implements CommonErrorCode {
    OK("0", "ok"),
    FAIL_MEMBER_INVALID("3001", "会员已过期"),
    FAIL_MEMBER_BLOCK("3002", "会员被冻结"),
    FAIL_MEMBER_NOT_FOUND("3003", "会员不存在");

    MemberErrorCode(String code, String msg) {
        this.code = String.format("%s%s", PREFIX_ERROR_CODE_MEMBER, code);
        this.msg = msg;
    }

    @Getter
    private final String code;
    @Getter
    private final String msg;
}

System.out.println(MemberErrorCode.FAIL_MEMBER_BLOCK);
System.out.println(MemberErrorCode.FAIL_MEMBER_NOT_FOUND);
```

输出结果：

```
MemberErrorCode.FAIL_MEMBER_BLOCK(code=2003002, msg=会员被冻结)
MemberErrorCode.FAIL_MEMBER_NOT_FOUND(code=2003003, msg=会员不存在)
```


