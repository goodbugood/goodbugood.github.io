# 《实现领域驱动设计》读书笔记


## 命名

> 对一个领域建模时，要仔细考虑什么样的对象做什么样的事情，这是对象行为的设计。对对象的命名能够传达准确的业务含义，即反映通用语言。
>
> 如果我们只是关注领域模型定义属性，提供getter和setter方法，你那只是创建纯粹的数据模型（DO）。

## 领域对象的特点

- 要对外暴露行为，尽可能少的暴露属性，对外暴露setter和getter方法

## 问答

### 如何确定领域对象中的行为方法的正确性？

最简单的方法就是看你封装的行为方法是否有业务交互。如果只是简单调用setter和getter方法。

## 我的思考

其实我们平时在建立领域模型时，也在可以暴露模型的行为方法，减少使用者对属性setter和getter操作的关注。但是我们的模型行为最大的缺点，就是职责不够单一。后期改动牵一发而动全身。

缺点就是封装了太多的单一职责的方法，没有面向过程式代码，一眼看下来便知道了整个业务流程执行了什么。而这种领域对象还要进入其内部才能了解具体做了什么。如果想要快速了解整个业务，流程图是必不可少的。