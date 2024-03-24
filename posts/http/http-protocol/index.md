# HTTP协议笔记


## HTTP Headers

我们经常能看到 header 头里的 `X-什么什么`的头，例如`X-PowerBy-PHP`，目前这种格式不被推崇。

> 自定专用消息头可通过'X-' 前缀来添加；但是这种用法被 IETF 在 2012 年 6 月发布的 [RFC6648](https://tools.ietf.org/html/rfc6648) 中明确弃用，原因是其会在非标准字段成为标准时造成不便；其他的消息头在 [IANA 注册表](https://www.iana.org/assignments/message-headers/perm-headers.html) 中列出，其原始内容在 [RFC 4229](https://tools.ietf.org/html/rfc4229) 中定义。 此外，IANA 还维护着[被提议的新 HTTP 消息头注册表](https://www.iana.org/assignments/message-headers/prov-headers.html)。

以上引用来源：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers

## 请求方法

### HEAD

> **HTTP `HEAD` 方法** 请求资源的头部信息，并且这些头部与 HTTP [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 方法请求时返回的一致。该请求方法的一个使用场景是在下载一个大文件前先获取其大小再决定是否要下载，以此可以节约带宽资源。

语法：

```ini
HEAD /index.html
```

### OPTIONS

> **HTTP 的 `OPTIONS 方法`** 用于获取目的资源所支持的通信选项。客户端可以对特定的 URL 使用 OPTIONS 方法，也可以对整站（通过将 URL 设置为“*”）使用该方法。

```ini
// 请求头
OPTIONS /index.html HTTP/1.1
OPTIONS * HTTP/1.1

// 响应头
HTTP/1.1 200 OK
Allow: OPTIONS, GET, HEAD, POST
```

## Content-Type

### 疑惑

关于消息类型中的编码类型，之前见到的都是`application/json; charset=utf-8`今天发现微信的网页调试工具，响应的是`application/json; encoding=utf-8`。

不知道`encoding`和`charset`两者区别是什么，还是HTTP协议都兼容。

![image-20220820105358699](/images/image-20220820105358699.png)

## 响应状态码

| status（状态码）          | reason（原因）                                               |
| ------------------------- | ------------------------------------------------------------ |
|                           |                                                              |
| 405 Method Not Allowed    | 就是说服务器不理解，不支持你这种请求方法                     |
| 500 Internal Server Error | 服务器业务代码出错                                           |
| 502 Bad Gateway           | 表示作为网关或代理的服务器，从上游服务器中接收到的响应是无效的 |
| 503 Service Unavailable   | 请求过多，服务器过载，服务不过来                             |
| 504 Gateway Timeout       | 表示扮演网关或者代理的服务器无法在规定的时间内获得想要的响应。<br />比如 Nginx 将请求转发给 PHP ，但是 PHP 未能在规定时间内进行响应。 |

## 常见问题

### 跨域问题

```ini
Access-Control-Allow-Origin: *
Access-Control-Allow-Origin: <origin>
```

例如，你允许任何网站跨域访问你的资源，那么可以在响应 header 头中添加`Access-Control-Allow-Origin: *`来解决。

如果只允许`https://www.qq.com`跨域访问，那么 header 头里添加`Access-Control-Allow-Origin: https://www.qq.com`即可。
