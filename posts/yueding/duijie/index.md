# 前后端对接中的问题


> 约定大于配置，配置大于代码。

## 0 和空字符串也是有意义的

==很多前后端在对接搜索接口时，总喜欢将字段传空字符串或者传 0 代表所有类型。这是不合适的。==

如果后端开发主导这么干，我认为是不合格的。

如果是前端主导这么做，我站在前端的位置是能够理解的他的想法的。因为他定义好了一个 model 数据表单对象，当用户选择全部时，他又需要从对象中剃掉一个属性，这无形中增加了他的工作量。

### 为何不能这么做

因为容易产生歧义。

因为在我们表字段中，0 有时候是有特殊含义的，他有可能代表某种状态。例如 0 在表中代表停用状态，你传来个 0 ，到底是让我输出所有状态的用户给你，还是仅返回停用状态的用户列表呢。这不就产生歧义了吗。

对于空字符串，新建表的时候，表中很多字符串字段，都喜欢配置一个默认值，即空字符串。如果这个字段使用了空字符串默认值，那么表中此字段必然会有很多空字符串。如果你给这个字段提交一个空字符串，那么我是返回表中所有列表给你还是仅返回此字段为空字符串的列表集合给你呢。

### 如何解决

那能不能不使用空字符串和 0 ，又能减少前端工作量呢？

可以，前后端可以自行定义其他类型代替全部即可。
