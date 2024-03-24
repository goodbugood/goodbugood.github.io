# 100条 sql


数据背景：

大家都在本地创建此背景 sql：

```sql

```

## 一对一携带一对多数据合并

背景：

> 查询出班级里的所有学生，多个学生的身份信息用逗号分割

| 学号 | 姓名 | 身份                       |
| ---- | ---- | -------------------------- |
| 1    | 张三 | 三好学生,学习委员,劳动班长 |
| 2    | 李四 | 体育委员,英语课代表        |

```sql
select id, name, group_concat(id_tag)
from user
inner join user_tag utag on utag.user_id = user.id
inner join tag on tag.id = utag.tag_id
group by user.id
```


