# 装饰者模式


## 前言

特点就是：一个接口，若干实现类，且每一个类的构造方法参数就是这个接口。他们互相构造，互相叠加。

## 代码展示

```java
// 各种包
class Bao {
    private String name;

    public Bao() {
    }

    public Bao(String name) {
        this.name = name;
    }

    public void diejia() {
        System.out.println(name);
    }
}

// 短信包
class Duanxin extends Bao {
    private Bao bao;

    public Duanxin(Bao bao) {
        this.bao = bao;
    }

    @Override
    public void diejia() {
        System.out.println("添加短信包1个");
        this.bao.diejia();
    }
}

// 流量包
class Liuliang extends Bao {
    private Bao bao;

    public Liuliang(Bao bao) {
        this.bao = bao;
    }

    @Override
    public void diejia() {
        System.out.println("添加流量包1个");
        this.bao.diejia();
    }
}

// 语音包
class Yuyin extends Bao {
    private Bao bao;

    public Yuyin(Bao bao) {
        this.bao = bao;
    }

    @Override
    public void diejia() {
        System.out.println("添加语音包1个");
        this.bao.diejia();
    }
}
```

应用

```java
// 先添加一个基本套餐：一般包含来电显示，国际漫游啥的
Bao bao = new Bao("中国移动神州行套餐");
Yuyin yuyin1 = new Yuyin(bao);
Yuyin yuyin2 = new Yuyin(yuyin1);
Duanxin duanxin = new Duanxin(yuyin2);
Liuliang liuliang = new Liuliang(duanxin);
liuliang.diejia();
```

执行结果，通过叠加，我们的一个神州行套餐就出来了，鉴于神州行打电话多，我们定制套餐时可以给2个语音包。这样灵活的定制便于我们定制其他套餐。

```java
添加流量包1个
添加短信包1个
添加语音包1个
添加语音包1个
中国移动神州行套餐
```

## 常见应用

Java 中的 IO 流就使用了装饰者模式。

比如我们在设计套餐组合，优惠叠加，都是其出现的场合。

## 疑惑

感觉Java中的代理模式跟装饰者模式很像，他们有哪些共同点和不同点，我该如何区分？

> 代理模式（Proxy Pattern）和装饰者模式（Decorator Pattern）在某些方面确实有一些相似之处，但它们的设计目的、使用场景和实现方式有所不同。以下是它们的共同点和不同点，以及如何区分它们：
>
> ### 共同点：
> 1. **增强对象的功能**：代理模式和装饰者模式都可以用来增强对象的功能，而不需要修改原始对象的代码。
> 2. **保持原始对象的接口**：两种模式都可以保持原始对象的接口不变，对外部调用者来说，无需知道代理或装饰者的存在。
>
> ### 不同点：
> 1. **设计目的**：
>    - **代理模式**：主要用于控制对对象的访问，可以在访问对象之前或之后执行一些额外的操作，例如权限控制、延迟加载等。
>    - **装饰者模式**：主要用于动态地给对象添加额外的职责，可以在不改变原始对象结构的情况下为对象添加新的行为。
>
> 2. **关注点**：
>    - **代理模式**：关注对对象的访问控制和增强，代理对象通常直接或间接地调用被代理对象。
>    - **装饰者模式**：关注在不改变接口的情况下为对象添加新的功能，装饰者对象通常包含被装饰对象的实例。
>
> 3. **使用场景**：
>    - **代理模式**：适用于需要控制对对象的访问、延迟加载、缓存等情况。
>    - **装饰者模式**：适用于需要动态地为对象添加新的功能、组合多个功能的情况。
>
> ### 区分方法：
> - 如果主要目的是控制对对象的访问或增强对象的行为，那么代理模式更适合。
> - 如果主要目的是动态地为对象添加新的功能而不改变接口，那么装饰者模式更适合。
>
> 总的来说，代理模式更注重对对象的访问控制和增强，而装饰者模式更注重为对象动态地添加新的行为。根据具体的需求和场景来选择适合的设计模式来解决问题。

