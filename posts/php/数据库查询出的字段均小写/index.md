# php 数据库查询字段全是小写


## TP3.2 框架查询数据库

查询数据，返回的字段全是小写，指定的小驼峰别名也被转成了小写。

因为 `\Think\Db\Driver` 默认写死的字段全小写。

![image-20230825182611291](/images/image-20230825182611291.png)

### 修改配置

```php
// config.php
"DB_PARAMS" => [
    \PDO::ATTR_CASE => \PDO::CASE_NATURAL,
],
```

## PDO 特性支持定义输出的结果集字段大小写

![060cd2224181a96c76e05fc5e5de840](/images/060cd2224181a96c76e05fc5e5de840.png)
