# 策略模式


## 概念

strategy

### 定义

策略模式：指的是定义一系列算法，将每一个算法封装起来，并让它们可以相互替换。策略模式让算法独立于使用它的客户而变化。

==策略模式仅仅封装算法==，提供新的算法插入到已有系统中，策略模式并不决定在何时使用何种算法。在什么情况下使用什么算法是由客户端决定的。

上下文类：印象中，一个策略接口，若干个策略实现类，他们还要与一个上下文类配合使用。其实上下文类就是聚合了策略类，封装方法调用策略的方法。

疑惑：既然调用策略实现类的方法能完成的任务，为何需要整个上下文类来间接调用呢。因为上下文类中，容易扩展其他功能。其次上下文类能帮我们封装一些其他复杂的业务，而调用者只需要跟上下文类打交道即可。

## 光说不练假把式

```java
// 手机开机策略
interface Shouji {
    public void kaiji();
}

// 小米手机开机策略实现类
class Xiaomi implements Shouji {
    @Override
    public void kaiji() {
        System.out.println("小米手机开机：美国有苹果，中国有小米");
    }
}

// 苹果手机开机策略实现类
class Pingguo implements Shouji {
    @Override
    public void kaiji() {
        System.out.println("苹果手机开机：apple");
    }
}
```

现在我们买一部手机很简单，只需要去官网买，回来开机就行了。

```java
Xiaomi xiaomi = new Xiaomi();
xiaomi.kaiji();
// 小米手机开机：美国有苹果，中国有小米
Pingguo pingguo = new Pingguo();
pingguo.kaiji();
// 苹果手机开机：apple
```

开机后我们发现，我们的手机还没有流量卡，还没有贴膜。这就是去官网下单的好处，什么都不送，理由环保。下次为了省心省力，我们决定去手机店买。

```java
// 手机店类就是一个上下文类
class ShoujiDian {
    private Shouji shouji;

    public ShoujiDian(Shouji shouji) {
        this.shouji = shouji;
    }

    public void kaiji() {
        System.out.println("手机贴膜完成");
        shouji.kaiji();
        System.out.println("插入手机卡");
    }
}

Xiaomi xiaomi = new Xiaomi();
ShoujiDian shoujiDian = new ShoujiDian(xiaomi);
shoujiDian.kaiji();
// 手机贴膜完成
// 小米手机开机：美国有苹果，中国有小米
// 插入手机卡
```

看完这里，应该明白为啥引入上下文类了吧，上下文类能够提供增值服务。

可能有的同学不以为然，任务手机开机策略实现类里也能实现贴膜，插流量卡啊。是的，那我今天跟你掰扯掰扯，下次不要跟我说没说哦，做人嘛，你吃肉，至少让大家有汤喝对吧。什么事你都干了，让别人怎么办？类也一样，单一职责。

## 应用场景

常见的策略应用场景：

- 对接三方支付，商户配置不同的平台，执行不同支付平台策略
- 对接三方发票服务商，商户配置不同的发票服务商，执行不同的平台开票策略
- 计算会员优惠，不同的会员等级，执行不同的优惠计算方式

