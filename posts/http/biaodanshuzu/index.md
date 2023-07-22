# 表单数组


[TOC]

在 HTTP 协议眼里只有字符串，没有所谓的表单数组，至于你要解析成表单数组，那是服务器代码的一厢情愿。

## 数据格式

HTML 表单发送数组数据：

```html
<form action="">
    <input type="text" name="user[0][name]" value="xiaoming">
    <input type="text" name="user[0][age]" value="20">
    <input type="text" name="user[1][name]" value="lihua">
    <input type="text" name="user[1][age]" value="22">
    <input type="text" name="user[2][name]" value="wanggang">
    <input type="text" name="user[2][age]" value="19">
</form>
```

![image-20220819104546817](./images/image-20220819104546817.png)

### PHP 接收到的数据

```php
// 打印获取到的数据
var_dump($_POST);
```

```php
array(1) {
  ["user"]=>
  array(3) {
    [0]=>
    array(2) {
      ["name"]=>
      string(8) "xiaoming"
      ["age"]=>
      string(2) "20"
    }
    [1]=>
    array(2) {
      ["name"]=>
      string(5) "lihua"
      ["age"]=>
      string(2) "22"
    }
    [2]=>
    array(2) {
      ["name"]=>
      string(8) "wanggang"
      ["age"]=>
      string(2) "19"
    }
  }
}
```

### Java 接收到的数据

```json
{
    "args": {},
    "data": "",
    "files": {},
    "form": {
        "user[0][age]": "20",
        "user[0][name]": "xiaoming",
        "user[1][age]": "22",
        "user[1][name]": "lihua",
        "user[2][age]": "19",
        "user[2][name]": "wanggang"
    },
    "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate, br",
        "Content-Length": "161",
        "Content-Type": "application/x-www-form-urlencoded",
        "Host": "httpbin.org",
        "Postman-Token": "97f65f22-18c0-404c-bf43-097274ec575d",
        "User-Agent": "PostmanRuntime/7.28.3",
        "X-Amzn-Trace-Id": "Root=1-62fef6e3-602efcd14413700f76a3a573"
    },
    "json": null,
    "origin": "113.111.187.215",
    "url": "https://httpbin.org/post"
}
```

### 问题

看到这里，大家会觉得，表单提交的就是数组啊，为何 PHP 拿到请求数据就能获得数组，而 Java 却不能呢。

需要强调的是，根本没有什么数组，HTTP 协议传输的就是字符串。上面的数据在 HTTP 报文中，其实传递的是这样的字符串。

![image-20220819110139914](./images/image-20220819110139914.png)

```text
user%5B0%5D%5Bname%5D=xiaoming&user%5B0%5D%5Bage%5D=20&user%5B1%5D%5Bname%5D=lihua&user%5B1%5D%5Bage%5D=22&user%5B2%5D%5Bname%5D=wanggang&user%5B2%5D%5Bage%5D=19
```

上面的数据还不够明显，因为请求参数被==urlencode==了，我解码下。

![image-20220819105341428](./images/image-20220819105341428.png)

```text
user[0][name]=xiaoming&user[0][age]=20&user[1][name]=lihua&user[1][age]=22&user[2][name]=wanggang&user[2][age]=19
```

==总结==：其实`user[0][name]`这种格式的数据就是字符串，只不过 PHP 故意对其进行处理，将其解析成了数组。
你甚至可以跟表单约定传`user.0.name`这种格式的数据，你将其解析成数组也是可以的。

### 验证

我们上面猜想 PHP 其实接到的参数就是字符串，没有所谓的数组。也可以编码进行验证。

```php
var_dump(file_get_contents('php://input'));
```

```text
string(113) "user[0][name]=xiaoming&user[0][age]=20&user[1][name]=lihua&user[1][age]=22&user[2][name]=wanggang&user[2][age]=19"
```
