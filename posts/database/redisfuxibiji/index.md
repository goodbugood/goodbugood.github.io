# Redis复习笔记


[TOC]

# Redis复习笔记

## 数据类型

| 数据类型                            | 特点                                                         | 添加                                                  | 删除 | 修改 | 查询                                               |
| ----------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------- | ---- | ---- | -------------------------------------------------- |
| string（字符串）                    | 二进制安全，可存图片<br />一个字符串最大能存512MB的内容      | set                                                   | del  | set  | get                                                |
| list（列表）                        | 列表里都是按插入顺序存储的字符串string类型<br />一个列表能存储2^32-1=42亿9496万7295个字符串 | rpush/lpush                                           |      |      | rpop/lpop                                          |
| set（无序集合）                     | 集合里的元素都是string类型<br />集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1) | sadd                                                  | srem |      | spop/srandmember/smembers                          |
| zset（有序集合）                    |                                                              | zadd<br />添加存在的元素时会返回 0 ，但是会更新 score | zrem |      | zrange                                             |
| hash（哈希表）                      | 一个哈希能存2^32-1=42亿9496万7295个键值对                    | hset                                                  | hdel |      | hget<br />hgetall 获取哈希表中的所有键值对         |
| publish/subscribe（发布/订阅）      |                                                              |                                                       |      |      |                                                    |
| stream                              | Redis 5.0 新增的数据结构<br />主要用于消息队列               |                                                       |      |      |                                                    |
| HyperLogLog（用来做基数统计的算法） | 基数统计就是统计不重复数据的个数                             | pfadd 基数表名 元素                                   |      |      | pfcount 基数表名<br />即可统计里面不重复元素的个数 |

### Redis Stream

官网说他是一种新型数据结构，Redis 5.0 才引入。可用来做消息队列，弥补`发布/订阅`数据结构的，因为发布订阅不支持消息持久化，断网宕机都会造成未被消费的消息丢失。

#### 特点

- 未被 ACK 的消息支持持久化
- 支持为消费者分组，类似于为队列 Queue 绑定 Topic
- 消费组负责消费消息，并移动游标`last_delivered_id`，每一个消费者同时用`pending_ids`维护消费组获取的消息但是没有消费完执行 ACK 的消息

#### 命令

- 发布消息

`xadd 文档名 记录的自增id 记录字段1 记录值1 记录字段2 记录值2`，`*`代表系统指定自增id，每次执行完，系统会返回新增记录的自增 id。

```sh
> xadd stream * name tony age 23
1662448868097-0
> xadd stream * color red status off
1662448921552-0
```

看看添加后的效果，就知道为啥将其类比`MongoDb`的文档而不是结构化数据库的表了。因为其数据没有`MySQl`之类的表字段结构限制。

![image-20220906153312040](./images/image-20220906153312040.png)

常用命令

| 命令                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ==xadd== stream_key * filed1 value1 ...                      | 添加记录                                                     |
| ==xlen== stream_key                                          | 获取 stream_key 流中记录的总数                               |
| ==xdel== stream_key 记录主键 id                              | 删除 stream_key 流中某个主键 id 对应的记录                   |
| ==xrange== stream_key - +                                    | 获取 stream_key 流中的所有记录，- 表示最小主键 id， + 表示最大 |
| ==xrange== stream_key - + count 3                            | 获取指定数量的 stream_key 流中的数据列表                     |
| ==xinfo stream== stream_key                                  | 查看 stream_key 的信息                                       |
| ==xgroup create== stream_key consumer1 $                     | 创建消费组 consumer1 绑定消息队列 stream_key                 |
| ==xreadgroup group== consumer1 cli ==count== 1 ==streams== stream_key > | 消费者读取消息                                               |
| ==xack== stream_key consumer1 1662453607729-0                | 消费者 ACK 消息 1662453607729-0                              |

- Redis 生成的自增 id 的含义

`1662448868097-0`其格式即`添加时的毫秒数-同毫秒时并发生成的第几条`。

## 应用场景

### 缓存

### 分布式锁

## 数据类型

### 字符串

### 队列

## 安全

大写为命令

### 检查 Redis 是否设置了密码

```sh
> CONFIG get requirepass
requirepass
```

### 设置密码

```sh
CONFIG set requirepass "abc123"
```

### 验证密码

```sh
AUTH "abc123"
```

## 分区

分区有点类似分布式，就是将 Redis 中的数据分到不同的 Redis 服务器上。

优点：

- 提高了内存和带宽

缺点：

- 分布在不同主机上 set 没法做集合运算。

### 分区类型

#### 水平分区

按照用户 id 所属的范围进行分区，使其分布在不同的 Redis 主机上。

#### Hash分区

先对 key 进行 hash 计算，得到的整数，再对主机数量取模运算，得出数据位于那台机器上。

例如：xiaoming_session_id 进行哈希运算得到整数 10，我们有 4 台 Redis 主机，10 % 4 = 2，所以我们应该去 Redis 主机编号为 2 的实例上去查询 xiaoming_session_id。

## 衍生问题

### 如何确保缓存与数据库数据的一致性

延迟双删：先删除缓存，更新数据库，完事再删除缓存。这种操作只能尽最大努力保证可靠，就像 TCP 3 次握手，4 次挥手一样，都是尽最大努力。

