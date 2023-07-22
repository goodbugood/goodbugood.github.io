# Servlet


> Servlet 是 Server Applet 的缩写，译为“服务器端小程序”，是一种使用 Java 语言来开发动态网站的技术。

> 使用原生 Java 开发动态网站非常麻烦，需要自己解析 HTTP 请求的报头，需要自己分析用户的请求参数，需要自己加载数据库组件，种种原因导致使用原生 Java 开发动态网站几乎是一件不能被接受的事情。正是基于这种原因，Java 官方后来推出了 Servlet 技术，它对开发动态网站需要使用的原生 Java API 进行了封装，形成了一套新的 API，称为 Servlet API。

> 您可能还听说过 Applet，它和 Servlet 是相对的：
>
> Java Servlet 是“服务器端小程序”，运行在服务器上，用来开发动态网站；
> Java Applet 是“客户端小程序”，一般被嵌入到 HTML 页面，运行在支持 Java 的浏览器中。
>
> Applet 和 Servlet 都是基于 Java 的一种技术，功能都非常强大，但是 Applet 开发步骤繁杂，而且只能在安装 Java 虚拟机（JVM）的计算机上运行，现在已经被 JavaScript 全面替代，几乎没有人再学习 Applet。

## 演进

Servlet 是第 1 代 Java Web 开发技术

JSP 是第 2 代 Java Web 开发技术，因为 Servlet 将 HTML 代码以字符串的形式向外输出，编写 HTML 文档就是在拼接字符串，非常麻烦

学习 JSP 的顺序，Java => Servlet => JSP

> JSP 才是现代化的 Web 开发技术，它允许 HTML 代码和 JSP 代码分离，让程序员能够在 HTML 文档中直接嵌入 JSP 代码。
>
> 现在没有人直接使用 Servlet 开发动态网站，大家都转向了 JSP 阵营。但是 JSP 依赖于 Servlet，用户访问 JSP 页面时，JSP 代码会被翻译成 Servlet 代码，最终，HTML 代码还是以字符串的形式向外输出的。您看，JSP 只是在 Servlet 的基础上做了进一步封装。
>
> JSP 代码可以调用 Servlet 类，程序员可以将部分功能在 Servlet 中实现，然后在 JSP 中调用即可。
>
> 总之，Servlet 是 JSP 的基础，Servlet 虽然不直接面向用户，但是它依然是 JSP 的后台支撑，想玩转 JSP，必须先玩转 Servlet。

## Servlet框架调用

![Servlet 处理HTTP请求](/images/1103524464-0.png)

## 转发与重定向的区别

转发由分发器`javax.servlet.RequestDispatcher#forward`触发。

重定向由`javax.servlet.http.HttpServletResponse#sendRedirect`触发。

二者虽然都能使用页面发生跳转，但是原理不同：

![image-20220828160923102](/images/image-20220828160923102.png)

## Session

![Session 工作原理图](/images/150219A04-0.jpg)

Servlet 容器为每一个浏览器创建一个 Session：

```java
javax.servlet.http.HttpServletRequest#getSession()
```

## 过滤器javax.servlet.http.HttpFilter

类似一个钩子，就是对请求内容做处理：权限检查，记录请求日志，甚至可以修改`javax.servlet.http.HttpServletRequest`中的内容。

也可以修改页面 XxxServlet 的响应`javax.servlet.http.HttpServletResponse`，比如在响应头里添加`X-Power-By: xxx公司`，过滤敏感词

```java
public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException;
```

## 监听器javax.servlet.http.HttpSessionListener

常见的几种监听器：

| 监听器                                                       | 触发场景                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ServletContextListener#contextInitialized<br />ServletContextListener#contextDestroyed | Servlet 容器初始化和容器关闭时触发 2 个对应方法              |
| HttpSessionListener                                          | 监听 HttpSession 的创建和销毁事件                            |
| ServletRequestListener                                       | 监听 ServletRequest 请求的创建和销毁事件                     |
| ServletRequestAttributeListener                              | 监听 ServletRequest 请求的属性变化事件（即调用`ServletRequest.setAttribute()`方法） |
| ServletContextAttributeListener                              | 监听 ServletContext 的属性变化事件（即调用`ServletContext.setAttribute()`方法） |

> ServletContext 还提供了动态添加 Servlet、Filter、Listener 等功能，它允许应用程序在运行期间动态添加一个组件，虽然这个功能不是很常用。

这是热加载技术吧。

监听器其实也是一个钩子，当执行相关标签方法时，就会触发对应的钩子方法。比如：

| 执行方法                                                     | 被触发方法                                              |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| 认证成功，创建session<br />javax.servlet.http.HttpServletRequest#getSession() | javax.servlet.http.HttpSessionListener#sessionCreated   |
| 退出登录，销毁session<br />javax.servlet.http.HttpSession#invalidate | javax.servlet.http.HttpSessionListener#sessionDestroyed |

## 疑问

Servlet 基于 Java 语言开发，运行 Servlet 代码只用 JRE 还不行，因为不支持 Servlet 规范。那所谓的 Servlet 容器不就是框架吗？

> 读者可能会提出疑问，我们自己编写的 Servlet 类为什么需要 Servlet 容器来管理呢？这是因为我们编写的 Servlet 类没有 main() 函数，不能独立运行，只能作为一个模块被载入到 Servlet 容器，然后由 Servlet 容器来实例化，并调用其中的方法。
