# 读知乎文章


> https://www.zhihu.com/question/30831266/answer/1595764766

基础的DDD四层架构：

```ini
用户界面层/接口层
    👇
    应用层
    👇
    领域层
    👇
    基础设施层
```

```ini
作者：阿里巴巴大淘宝技术
链接：https://www.zhihu.com/question/30831266/answer/1595764766
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

-- bootstrap
    -- BeanConfig
-- application / 应用层
    -- pv
        -- ChannelPvApplicationService
    -- sns
-- domain / 领域层
    -- abtest
        -- AbtestService
    -- address
    -- coupon
        -- entity
            -- Coupon
            -- CouponStatus
            -- CategoryCouponTemplate
    -- category
    -- user
        -- UserRepository
        -- service
            -- OneIdService
            -- UserService
    -- item
        -- ItemRepostory
    -- live
        -- LiveStatus
-- infrastructure / 基础设施层
    -- concurrent
        -- ThreadPoolExecutorFactory
        -- MonitorableCallerRunsPolicy
    -- dal
        -- IGraphDal
        -- TuringDal
        -- DefaultUserRepository
    -- dao
        -- MybatisItemDao
    -- util
        -- DateUtil
        -- MoneyUtil
        -- UriUtil
    -- monitor
        -- Event
        -- Timing
        -- TimingAspect
        -- TimingEvent
        -- Monitors
-- view / 接口层、用户界面层
    -- atomicwidget
        -- BannerWidget
        -- CrazySubsidyWidget
        -- FeedItemsWidget
        -- NavigateBarWidget
        -- LiveWidget
    -- page
        -- HomeScreenPage
        -- CategoryFeedsPage
        -- SearchCardPage
    -- widget
        -- Widget
        -- DispatchableWidget
        -- Debuggable
        -- AbstractWidget
        -- AbstractDispatchableWidget
        -- WidgetDispatcher
        -- WidgetResult
        -- WidgetContextIncompatibleException
```

总结：

| 层         | 涵盖                               |
| ---------- | ---------------------------------- |
| 接口层     |                                    |
| 应用层     |                                    |
| 领域层     | domain目录下按领域划分的目录，     |
| 基础设施层 | 事件库/工具util/dao层的模型/线程池 |

> 领域对象、值对象、DTO、Service等定义都放在子域的包下，不要有大而全的entity、service、impl等包(这里的子域是一个内聚的逻辑概念，对应的是领域设计里的子域，如上例中的item在我们的导购里就是商品这个子域)
>
> 
>
> 常量定义尽量跟着相关的类走，作为类的静态字段，不要有大而全的Constant类(Switch相关的除外，但也要按职责尽量拆分开关类)
>
> 
>
> 作者：阿里巴巴大淘宝技术
> 链接：https://www.zhihu.com/question/30831266/answer/1595764766
> 来源：知乎
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 命名规范

- 领域对象不带后缀
- DTO，RPC 接口提供的对象
- VO，跟前端交互的对象
- PO，跟数据库直接交互的对象

## 注释

注释要说明为什么这么做，而不是在做什么。毕竟看了你代码还不知道你在做什么的话，那你代码写的太烂了。

## 面向对象编码

- 遵循原则：SRP/OCP/LSP/ISP/DIP
- 尽量只暴露行为，不暴露数据
- 慎用继承，优先使用组合方式

## 日志

- 后台一定要有操作日志，数据变更日志
- 日志要配置异步写盘

## 数据结构使用

- 数组的使用，无随机读取需求时，使用`LinkedList`代替`ArrayList`

## 编码

- 功能性代码与非功能性代码要分离
- 时刻警惕 NPE，多用 Optional 处理

## 分层

![image-20220824110658236](./images/image-20220824110658236.png)

## COLA

基于 DDD 的业务代码架构的最佳实践。
