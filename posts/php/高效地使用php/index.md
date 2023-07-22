# 教你高效地使用PHP


## 字符串初始化尽量使用单引号

单引号不需要检查解析字符串中的变量，效率高于双引号。就类似别人告诉不需要线程安全的场景下，变长字符串让你使用 `StringBuilder` 而不是 `String` 一个道理。

## 过长 SQL 定义尽量使用 heredoc

### 区别

#### Heredoc 类似双引号，解析变量

```php
$userId = 123;
$sql = <<<EOT
select id from user where user_id = $userId limit 1
EOT;

// PHP 5.3.0 开始支持双引号
$userId = 123;
$sql = <<<"EOT"
select id from user where user_id = $userId limit 1
EOT;
```

输出结果：

```
select id from user where user_id = 123 limit 1
```

#### Nowdoc 类似单引号，不解析变量

PHP 5.3.0 版本开始增加 Nowdoc

```php
$userId = 123;
$sql = <<<'EOT'
select id from user where user_id = $userId limit 1
EOT;
```

输出结果：

```
select id from user where user_id = $userId limit 1
```

## PHP 内置 Web 服务器

```php
// 监听 ipv4 地址
php -S 0.0.0.0:80 -t web服务器目录

// 监听 ipv6 地址
php -S [::0]:80 -t web服务器目录
```

### 参考

- https://www.php.net/manual/zh/features.commandline.webserver.php

