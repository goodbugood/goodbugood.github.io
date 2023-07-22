# 模板模式


父类设计一个算法需要哪几步，子类则实现这些步骤。

## 大咖秀

### javax.servlet.http.HttpServlet

抽象类 javax.servlet.http.HttpServlet 在其方法`service()`定义了处理 get 请求，post 请求。而我们只需要继承 HttpServlet ，并重写 `doGet`和`doPost`方法即可。

> 由于我们使用的请求方式主要是 GET 和 POST，所以通过继承 HttpServlet 类创建 Servlet 时，只需要重写 doGet 或者 doPost 方法

