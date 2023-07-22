# php工程归档文件phar的打包和运行


## 目录结构

![image-20230613093224878](/images/image-20230613093224878.png)

## phar归档文件构建脚本

```php
<?php
// build.php

// 打包的文件名
$pharFileName = 'hellophar.phar';

// 打包是追加模式，为了保证打包里内容最新，先删除之前的包
file_exists($pharFileName) && unlink($pharFileName);

$phar = new Phar($pharFileName, 0, $pharFileName);

$dir = dirname(__FILE__);

// 打包 src 目录里面的内容，不包含 src 目录本身
$phar->buildFromDirectory($dir . '/src');

// main.php 执行 .phar 文件时执行的，index.php web 服务器入口文件
$phar->setDefaultStub('main.php', 'index.php');
```

## phar运行时的入口文件

入口文件类似我们 C 语言的 `main.c` ，Java 中的 `main` 方法。

```php
<?php
// main.php

// 加载工具方法文件
require 'function.php';

// phar 其实是一个压缩包，要执行必须解压，定义解压的目录
$path = sprintf('%s/pharextract/%s', Extract_Phar::tmpdir(), 'hellophar');
// 通常的解压路径：C:\Users\用户名\AppData\Local\Temp\pharextract\phar项目名称
$path = realpath($path);

// 为了方便了解应用被解压的目录，我们打印出来
echo sprintf("Application Dir: %s\n", $path);

// 解压目录不存在，则解压 hellophar.phar
is_dir($path) || Extract_Phar::go(false);

// 解析命令
$host = parse_cmd($argv, ['-h', '--host'], '127.0.0.1');
$port = intval(parse_cmd($argv, ['-p', '--port'], 80));

// 在解压目录里运行 php 内置 http 服务器
$cmd = sprintf('php -S %s:%d -t %s', $host, $port, $path);
exec($cmd);
// echo shell_exec($cmd), PHP_EOL;
```

## 提交表单

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="resources/css/bootstrap.min.css">
    <title>home</title>
</head>
<body>
<div class="container">
    <div class="row">
        <h1>php app</h1>
        <div class="row">
            <form method="post" action="post.php">
                <div class="mb-3">
                    <label for="username" class="form-label">Username</label>
                    <input type="text" class="form-control" id="username">
                    <div id="emailHelp" class="form-text">please input your name.</div>
                </div>
                <button type="submit" class="btn btn-primary" onclick="return;">Submit</button>
            </form>
        </div>
    </div>
</div>
</body>
</html>
```

## post文件

```php
<?php

date_default_timezone_set('Asia/Shanghai');

$now = date('Y-m-d H:i:s');

exit($now);
```


