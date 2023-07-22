# 前言


聊聊设计模式

| 分类   | 模式                                           |
| ------ | ---------------------------------------------- |
| 创建型 | 简单工厂，工厂方法，抽象工厂<br />建造者，原型 |
| 结构型 | 适配器，装饰者，代理，外观，享元               |
| 行为型 | 策略                                           |

![image-20220823191726542](./images/image-20220823191726542.png)

## 特点

### 单例singleton

3私1公2静态：

| 概念  | 具体                                                         |
| ----- | ------------------------------------------------------------ |
| 3私   | 私有的 singleton 属性<br />私有的构造方法，防止 new<br />私钥的 clone 方法，防止对象被克隆 |
| 1公   | 公开的 getInstance() 方法                                    |
| 2静态 | 静态的 singleton 属性<br />静态的 getInstance() 方法         |

| 方式       | 线程安全                                                     | 非线程安全                              |
| ---------- | ------------------------------------------------------------ | --------------------------------------- |
| 懒汉式     | getInstance() 方法加同步 synchronized                        | getInstance() 方法不加同步 synchronized |
| 饿汉式     | 加载类文件时，实例化单例<br />private static Singleton instance = new Singleton(); | 不存在非线程安全                        |
| 静态内部类 | private static class SingletonHolder {<br /><br/>    private static final Singleton INSTANCE = new Singleton();<br /><br/>} |                                         |

### 策略模式

算法策略接口，算法策略实现类，策略上下文类。

算法策略实现类实现算法策略接口，策略上线文类，其属性聚合算法策略，并二次实现算法策略。

### 观察者模式

2个接口，观察者接口`Observer`和被观察者接口`Subject`。被观察者属性聚合观察者。

```php
// 观察者
Observer {
    /* 方法 */
    abstract public update(Subject $subject): void
}

// 被观察者
Subject {
    private $observers = array();

    /* 方法 */
    abstract public attach(Observer $observer): void
    abstract public detach(Observer $observer): void
    abstract public notify(): void
}
```

使用方式就是被观察者发生改变时，调用`notify`方法遍历`$observers`执行其`update`方法。

> 逻辑是一种能力，而套路是方法论、是经验。逻辑是道的东西，而方法论是术的东西。

设计模式就是套路，就是方法论。
