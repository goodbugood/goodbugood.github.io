# Redis使用过程中存在的问题


## Scan使用中存在的问题

```php
public function scan(&$iterator, $pattern = null, $count = 0)
```

- 游标 $iterator 初始值为 null，当其返回值为 0 时表示整个迭代结束。
- $count 是每次迭代遍历字典槽的数量（约等于），可认为是每次迭代返回元素的最大个数，并不是指定返回元素的数量

存在的问题：

如果指定的 COUNT 过小，整个迭代过程会很长。
