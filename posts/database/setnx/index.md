# Redis的分布式锁


```sh
setnx key value
```

`setnx`命令的全称是`set if not exists`，即当指定的 key 不存在时，设置值并返回 1，如果存在什么都不做，并返回 0。

setnx实现分布式锁

## 方式一

```php
// 锁名词
$key = 'buy_order_lock';
// 加锁
if (false === $redis->setnx($key, time())) {
    var_dump('锁被其他进程拿到，且未超时');
    return;
}
// 获得锁
// 执行业务 ...
// 释放锁
if (0 === $redis->del($key)) {
    var_dump('任务完成，释放锁失败');
}
```

==存在问题==：

一旦释放锁失败，就会造成死锁。

## 方式二

```php
// 加锁
if (false === $redis->setnx($key, time())) {
    var_dump('锁被其他进程拿到，且未超时');
    return;
}
// 获得锁
// 执行业务 ...
// 释放锁
if (0 === $redis->del($key)) {
    var_dump('任务完成，释放锁失败');
    if (false === $redis->expire($key, 0)) {
        var_dump('任务完成，设置锁过期时间失败');
    }
}
```

==存在问题==：

释放锁失败，再给锁加个过期时间，这种做法跟释放锁失败，再次执行释放锁没啥区别。还是没有解决死锁的问题。

这种释放锁失败的问题没法避免，不如给锁加个超时时间。

## 方式三

```php
// 锁名词
$key = 'buy_order_lock';
// 锁超时时间
$timeout = 10;
// 加锁
if (false === $redis->setnx($key, time() + $timeout)) {
    $t = $redis->get($key);
    if ($t > time()) {
        var_dump('锁被其他进程拿到，且未超时');
        return;
    }
    // 释放超时锁
    if (0 === $redis->delete($key)) {// 临界区
        var_dump('释放超时锁失败');
        return;
    }
    // 再次加锁
    if (false === $redis->setnx($key, time() + $timeout)) {
        var_dump('再次加锁失败');
        return;
    }
}
// 获得锁
// 执行业务 ...
// 释放锁
if (0 === $redis->del($key)) {
    var_dump('任务完成，释放锁失败');
}
```

解决了死锁的问题，但是又引入了新的问题。

==存在问题==：

如果进程 P2 和 P3 同时进入临界区，进程 P2 删除了锁，并获得了锁，但是 P3 仅接着又删除了锁，再次获得了锁。

这种并发问题又出现了，因为又有多个进程获得了锁。

但是需要注意另一个问题，超时时间一定要合理，例如超时时间设置过短，任务还没执行完，锁已经被超时释放了。其他进程会获得锁。

超时时间又不能太长，因为一旦出现释放锁失败，锁又未超时，会导致锁一直无法获取。

## 改进方式四

使用`getset`命令

```php
// 加锁
if (false === $redis->setnx($key, time() + $timeout)) {
    $t1 = $redis->get($key);
    if ($t1 > time()) {
        var_dump('锁被其他进程拿到，且未超时');
        return;
    }
    // 这个命令其实是两步操作：1，返回锁旧的超时时间；2，设置锁新的超时时间
    $t2 = $redis->getSet($key, time() + $timeout);// 临界区
    if ($t2 != $t1) {
        var_dump('锁虽然超时，但是刚被别人获取了');
        return;
    }
}
// 获得锁
// 执行业务 ...
// 释放锁
if (0 === $redis->del($key)) {
    var_dump('任务完成，释放锁失败');
}
```

==存在问题==：

例如 P1 进程释放锁失败，最后锁超时。

P2 和 P3 进程同时进入临界区，P3 进程先执行了`getset`命令，P2 进程后执行了`getset`命令，P2 进程此时获得时间是 P3 设置的超时时间，且大于当前时间，所以 P2 进程获得锁失败。

可是有没有发现一个问题，==P3 进程锁超时的时间被 P2 进程给修改了==。不过没有关系。

如果能将上面的`getset`命令和`expire`命令合成一个原子命令。那么就能解决设置过期时间失败，释放锁失败造成的死锁问题。

## 方式五

```php
// 加锁
if (false === $redis->set($key, 1, ['nx', 'ex' => $timeout,])) {
    var_dump('锁被其他进程拿到，且未超时');
    return;
}
// 获得锁
// 执行业务 ...
// 释放锁
if (0 === $redis->del($key)) {
    var_dump('任务完成，释放锁失败');
}
```

这种方式简单，还好用，当然是是这个命令帮我们封装了一些操作。

==存在问题==：

释放锁这里是一直存在的问题，P1 进程设置的锁如果超时了，P2 进程获得了锁，这时候 P1 进程去释放锁，就会将 P2 进程的锁给释放了，误删了。

那我们能不能在加锁的同时，每个进程放置 1 个 token，释放锁之前检查锁的 token 是不是自己之前设置的，以免释放了别人的锁。

## 方式六

避免 del 释放其他进程的锁，del 之前应该先检查锁的密码是否是当前进程设置的。

执行`get`和`del`又会出现临界区，可能`get`的时候密码正确，`del`的时候，锁已经因为超时其他进程抢占了，造成释放其他进程的锁。

那么如果让`get`和`del`能够原子操作就可以了。

Eedis 执行命令是单线程的，毕竟 IO 不是他的瓶颈。而且 Redis 服务器会单线程原子性执行 Lua 脚本，保证 Lua 脚本在处理的过程中不会被任意其它请求打断。

```php
// 锁名词
$key = 'buy_order_lock';
// 锁超时时间
$timeout = 10;
$password = 'abc123';
// 加锁
if (false === $redis->set($key, $password, ['nx', 'ex' => $timeout,])) {
    var_dump('锁被其他进程拿到，且未超时');
    return;
}
// 获得锁
// 执行业务 ...
// 释放锁
$delScript = <<<'LUA'
if ARGV[1] == redis.call("get", KEYS[1])
then
    return redis.call("del", KEYS[1])
else
    return -1;
end
LUA;
// 脚本错误返回字符串
$res = $redis->eval($delScript, [$key, $password], 1);
if (false === $res) {
    var_dump('lua 脚本有问题');
    var_dump($redis->getLastError());
} elseif (-1 === $res) {
    var_dump('任务完成，释放锁失败，锁超时，被其他进程获得，无需释放');
} elseif (1 !== $res) {
    var_dump('释放锁失败');
}
```

## 推荐方式

方式六

## 举一反三

### 线程锁

todo

### 同一操作系统内的进程锁

todo
