# 编程规约


约定大于配置，配置大于编码。

## 关键字顺序

| 示例                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| @Override public ==abstract== void service(ServletRequest req, ServletResponse res)         throws ServletException, IOException; | 抽象方法                                                     |
| private static final long serialVersionUID = 1L;<br />private static final long serialVersionUID = 8294180014912103005L; | 类常量，非大写                                               |
| private static ==final== String METHOD_POST = "POST";        | 类常量，大写                                                 |
| public static void main(String[] args) {}                    | main方法                                                     |
| private ==transient== String password;                       | 禁止序列化某个属性，这样能保证序列化后存放的内容里无 transient 修饰的内容 |
| public ==default== void sessionDestroyed(HttpSessionEvent se) {} | JDK1.8 引入，接口的默认实现，空方法体                        |
| protected void finalize() throws Throwable {}                | 类的析构方法                                                 |
| public final ==synchronized== void setName(String name) {}   | 设置线程名称                                                 |
| private ==volatile== boolean flag = false;                   | 使用 volatile 修饰线程共享变量                               |
| public ==synchronized== StringBuffer append(String str) {}   | 同步方法                                                     |

总结顺序：[public|protected|private] [static] [final|default|transient|abstract] [synchronized] [返回值类型|变量类型]

- 第一个修饰符必须是访问修饰符 public|protected|private
- 最后一个修饰符必须是变量类型/返回值类型修饰符
- static final
- final synchronized

## 方法起名

- 见名知意 > 方法名长度

| 业务                 | 方法名                    | 名画名嘴                                                     |
| -------------------- | ------------------------- | ------------------------------------------------------------ |
| 建造者模式的建造输出 | build()                   |                                                              |
| 获取当前类的实例     | getInstance()             |                                                              |
| 获取当前类的单例     | getSingleton()            | SLF4J框架中的`org/slf4j/impl/StaticLoggerBinder.getSingleton()` |
| Dao层查询数据        | findByXxx() / selectByXxx |                                                              |
| Service层查询数据    | getXxx() / listXxx()      |                                                              |

### 增删改查

|业务|方法前缀|
|-|-|
|增|createXXX()|
|删|基础层使用 deleteXXX()，业务层使用 removeXXX()|
|改|updateXXX()|
|查|查 1 条 findOne()/查多条 findAll()|

### 不同层的区别

在 infrastructure 基础层，下单就是`createOrder`很简单明了，但是在业务层，就应该按业务术语来，而不是有 CURD 的影子，例如叫`placeOrder`。

取消订单，基础层就是`deleteOrder`，业务层`cancelOrder`。

> Tips：在业务层，尽量避免 CRUD，努力找到更好的业务词汇来表达业务语义，如果非用不可，请使用约定好的 CRUD 命名。

## 大咖秀

我们在规范自己代码风格的同时，还要有一颗包容的♥。

### 不整洁的代码

Java 源码 `java.lang.Class`，产于 1994, 2014

![image-20220907202327995](./images/image-20220907202327995.png)

## 应用设计

在设计开发一个系统时，可以分为三层，各层关注点不同：

高层：架构设计
中层：设计原则、设计模式
底层：代码整洁之道

## 常量写左边的好处

来自 chatgpt 的回答：

> 1. 避免赋值误写为比较：将常量写在左边可以避免在比较时将赋值操作误写为比较操作，因为在大多数编程语言中，赋值操作的返回值是被赋的值，而不是布尔值。
> 2. 提高代码可读性：将常量写在左边可以使代码更易读，因为常量通常是程序员自己定义的，而变量则可能是从其他地方传入的，将常量写在左边可以更清晰地表达程序员的意图。
> 3. 提高代码效率：将常量写在左边可以提高代码的执行效率，因为在大多数编程语言中，将常量写在左边可以避免进行类型转换，从而减少了代码的执行时间。

