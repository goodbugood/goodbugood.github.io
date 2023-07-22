# SQL一百问


## 语句

### BETWEEN字句有顺序区别吗

```sql
# 语句 1
select id form t1 between '2022-10-01' and '2022-10-07';

# 语句 2
select id form t1 between '2022-10-07' and '2022-10-01';
```

- 语句 1 与语句 2 有区别吗？

有，语句 2 只会返回 null。

- between 是闭区间吗？

闭区间。
