## insert语法

```
insert into 表(字段,...) values('对应字段值',''...)


特点：
1. insert into支持插入多行，支持子查询插入
2. 要求多条查询语句的查询的类型和顺序一致
3. union关键字默认去重，使用union all可以包含重复项
```