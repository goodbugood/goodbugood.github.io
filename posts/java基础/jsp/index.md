# JSP


JSP（Java Server Pages）

优点类似 PHP 中的模板引擎，既然是模板引擎，那就要按照模板引擎的一套规范（语法）来编写页面，最后进入 Servlet 容器执行前，将其替换（有的地方说是编译）转成 Servlet 类，你要搞清楚，Servlet 容器只能运行 Servlet 类，他可不认识 JSP 语法，毕竟那都不是 Java 语言格式，你让 JVM 怎么跑。

或者你称他是 Java 开发网页的模板语言。

我认为是这样的一个运行流程。

JSP（替换成）-> Servlet类（挂载到）-> Servlet容器（编译成）-> Java字节码 -> JVM（跑起来）

## 疑问

- 为啥引入 JSP

```java
java.io.PrintWriter pw = javax.servlet.http.HttpServletResponse#getWriter();
pw.write("<html><body><h1>");
pw.write("hello world");
pw.write("</h1></body></html>");
pw.flush();
```

Java 适合写业务逻辑，可不适合拼接 HTML 格式文件给前端。就像 PHP 为了解决开发 Web 页面过慢，而引入模板引擎。

一个 Web 页面的静态内容比动态内容多，Java 适合输出动态内容，如果是动态内容多，静态内容少，就直接使用 Servlet 开发了。

- 为啥说页面的Servlet挂载到Servlet容器呢？

因为我觉得页面的 Servlet 类就是一个钩子，页面只需要实现 Servlet 容器要调用的 `javax.servlet.http.HttpServlet#service(javax.servlet.http.HttpServletRequest, javax.servlet.http.HttpServletResponse)`方法即可。
