# MyBatisPlus使用笔记


## 注解

### 表实体注解

#### 表名起别名

注解在实体类上，即此类映射库里的某个表名。

```java
// 表 user_info 映射到实体 User
@TableName(value = "user_info")
public class User {}
```

#### 表字段起别名

```java
// 主键字段起别名
@TableId(value = "id_number")
private Integer id;

// 普通字段起别名
@TableField(value = "xingming")
private String userName;
```

#### 字段入库转 JSON 串

将注解注解在要转 json 的字段上。

```java
import com.baomidou.mybatisplus.extension.handlers.JacksonTypeHandler;

@TableField(typeHandler = FastjsonTypeHandler.class)
private InnerClass innerClass;
```

## 添加时间，更新时间使用数据字段填充功能

### 步骤

1. 给时间字段添加填充场景，`@TableField(fill = FieldFill.INSERT)`或`@TableField(fill = FieldFill.INSERT_UPDATE)`
2. 定义 SpringBoot 组件`@Component`，实现`MetaObjectHandler`

## 参考

- [MyBatis-Plus事务管理](https://blog.csdn.net/weixin_44141870/article/details/116048223)
