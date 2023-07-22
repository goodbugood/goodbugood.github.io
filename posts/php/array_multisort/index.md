# PHP二维数组排序


## 问 chatgpt

![image-20230625175725247](/images/image-20230625175725247.png)

## 实践出真知

```php
public function testArrayMultiSort()
{
    $data1 = array(
        array('id' => 1, 'name' => 'Tom', 'age' => 25),
        array('id' => 2, 'name' => 'Alice', 'age' => 22),
        array('id' => 3, 'name' => 'Bob', 'age' => 27),
    );
    $data2 = array(
        array('id' => 1, 'name' => 'Tom', 'age' => 25),
        array('id' => 2, 'name' => 'Alice', 'age' => 22),
        array('id' => 4, 'name' => 'Alice', 'age' => null),
        array('id' => 3, 'name' => 'Bob', 'age' => 27)
    );
    $data3 = array(
        array('id' => 1, 'name' => 'Tom', 'age' => 25),
        array('id' => 2, 'name' => 'Alice', 'age' => 22),
        array('id' => 4, 'name' => 'Alice',),
        array('id' => 3, 'name' => 'Bob', 'age' => 27)
    );
    $ages = array_column($data1, 'age');
    array_multisort($ages, SORT_DESC, $data1);
    self::assertSame(27, $data1[0]['age']);
    $ages = array_column($data2, 'age');
    array_multisort($ages, SORT_DESC, $data2);
    self::assertSame(27, $data2[0]['age']);
    $ages = array_column($data3, 'age');
    array_multisort($ages, SORT_DESC, $data3);
    self::assertSame(25, $data3[0]['age']);
    self::assertSame(22, $data3[1]['age']);
    self::assertSame(27, $data3[3]['age']);
}
```

使用 `array_multisort` 需要注意安全，当二维数组中没有指定的排序字段时，排序功能就失效了。


