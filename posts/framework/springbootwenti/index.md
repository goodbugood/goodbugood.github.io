# SpringBoot问题收集


## Controller层问题

### Request method 'GET' not supported

使用 postman 发送请求，明明发送的是 `POST` 请求，却报 `Request method 'GET' not supported`。而且注解使用的就是 `@PostMapping`，有点丈二和尚摸不着头脑了。

网络搜索了解，是因为你访问的是 `http`，Nginx 将 `http` 请求 302 重定向到了 `https`，而且重定向发的是 `GET` 请求，所以报此路由不支持 `GET` 请求。

#### 参考：

- [使用postman发送post请求，却报错不支持get请求的原因](https://blog.csdn.net/a624193873/article/details/107739640)
