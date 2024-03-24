# 状态模式建模与实现


## 简述

名称：状态模式

关键字：State

状态模式是一种行为型设计模式，它允许一个对象在其内部状态改变时改变其行为，看起来就像是改变了对象的类。

一个类通过状态基类完成工作，并将这份工作委托给状态的行为去完成。也就是当类的状态变化时，委托的对象也会改变。

## 代码示例

> 在订单状态发生改变时，可以定义不同的状态类来表示不同的订单状态，每个状态类包含该状态下的业务处理逻辑。订单类则持有当前订单状态的引用，并委托给当前状态类来处理具体的业务逻辑。
>
> 简要理由如下：
> 1. **清晰的状态划分**：状态模式将不同状态的行为逻辑进行了清晰的划分，使得代码结构更加清晰和易于理解。
> 2. **易于扩展**：当需要新增订单状态或者修改状态的业务逻辑时，只需要添加新的状态类或修改现有状态类的行为，而不需要修改订单类的代码，符合开闭原则。
> 3. **避免大量的条件判断**：状态模式避免了大量的条件判断语句，提高了代码的可维护性和可扩展性。
>
> 下面是一个简单的状态模式示例代码：

```java
// 订单状态接口
public interface OrderState {
    void processOrder(Order order);
}

// 待支付状态
public class PendingPaymentState implements OrderState {
    @Override
    public void processOrder(Order order) {
        // 处理待支付状态的业务逻辑
    }
}

// 已支付状态
public class PaidState implements OrderState {
    @Override
    public void processOrder(Order order) {
        // 处理已支付状态的业务逻辑
    }
}

// 订单类
public class Order {
    private OrderState currentState;

    public void setState(OrderState state) {
        this.currentState = state;
    }

    public void processOrder() {
        currentState.processOrder(this);
    }
}

// 使用示例
public class Main {
    public static void main(String[] args) {
        Order order = new Order();
        
        // 根据订单状态不同执行不同的业务逻辑
        order.setState(new PendingPaymentState());
        order.processOrder();
        
        order.setState(new PaidState());
        order.processOrder();
    }
}
```

> 通过使用状态模式，可以更好地组织订单状态与业务逻辑之间的关系，使代码更加清晰和易于维护。

## 疑惑

感觉Java中的策略模式跟状态模式很像，他们有哪些共同点和不同点，我该如何区分？

以下来自 chatgpt 的回答：

> 策略模式（Strategy Pattern）和状态模式（State Pattern）在某种程度上确实有一些相似之处，但它们的设计目的和应用场景有所不同。以下是它们的共同点和不同点，以及如何区分它们：
>
> ### 共同点：
> 1. **行为的封装**：策略模式和状态模式都涉及将特定行为封装成独立的类，使得这些行为可以独立变化并在运行时动态切换。
> 2. **减少条件语句**：两种模式都有助于减少大量的条件语句，提高代码的可维护性和可扩展性。
>
> ### 不同点：
> 1. **设计目的**：
>    - **策略模式**：主要关注的是算法的选择和切换，允许客户端在运行时选择算法。
>    - **状态模式**：主要关注的是对象的状态切换和状态所产生的行为变化，使得对象在不同状态下表现不同的行为。
>
> 2. **使用场景**：
>    - **策略模式**：适用于需要在运行时动态切换算法的情况，比如支付方式的选择、排序算法的选择等。
>    - **状态模式**：适用于对象在不同状态下具有不同行为的情况，比如订单状态的变化、文档编辑器中的不同编辑状态等。
>
> 3. **关注点**：
>    - **策略模式**：关注的是不同算法之间的差异，每个算法是相对独立的。
>    - **状态模式**：关注的是对象状态的变化，不同状态下对象的行为可能会有重叠。
>
> ### 区分方法：
> - 如果问题的关键在于不同的算法选择和切换，那么策略模式更适合。
> - 如果问题的关键在于对象状态的变化，不同状态下对象行为的变化，那么状态模式更适合。
>
> 总的来说，策略模式更注重算法的选择，状态模式更注重对象状态的变化。根据具体的业务需求和问题特点，选择适合的设计模式来解决问题。

