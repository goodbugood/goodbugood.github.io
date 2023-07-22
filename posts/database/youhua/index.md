# MySQL优化


## SQL优化

### Using index condition 与 Using index 的区别

using index 代表使用覆盖索引，不用回表。

而 using index condition 表示仅靠索引返回的数据还不够，还需要回表，不过回表前会过滤 where 条件。

## 索引

### 指导 MySQL 使用索引

- force index(索引名称)

强制 mysql 优化器使用索引，没有商量余地。

- use index(索引名称)

建议 mysql 优化器，使用我们建议的索引，只是建议不代表真的使用。

- ignore index(索引名称)

强制 mysql 优化器不使用某个索引，即忽略此索引。

## 分库

### 逻辑分库

根据群里讨论，逻辑分库是指单服务器上，创建多个 database 的分库。

关于逻辑分库的性能讨论

### 物理分库
