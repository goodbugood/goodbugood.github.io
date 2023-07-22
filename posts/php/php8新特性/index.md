# PHP8.0引入nullsafe


## ??

```php
$name = null;

$name = isset($name) ? $name : '';
```

等效：

```php
$name = null;

$name = $name ?? '';
```

## ?:

```php
$name = ' ';

$name = empty($name) ? '' : $name;
```

等效：

```php
$name = ' ';

$name = $name ?: '';
```


