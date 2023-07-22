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

