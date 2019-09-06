## insert语法

```
方法一：
insert into 表(字段,...) values('对应字段值1',''...)
1. 插入值与列类型一致或者兼容
2. 不可以为null的列必须插入值，可以为null值可以插入或者不插入
3. 列名的顺序可以调换
4. 列数和值的个数必须一致
5. 可以省略列名，而且列的顺序和表的顺序一致

方法二：
2. insert into 表  
set 列名=值,列名=值,....

特点：
1. insert into支持插入多行，支持子查询插入
2. insert into支持子查询
   insert into 表(字段,...)
   select (字段,...) from 表
   insert into 表
   select '值1','值2'....
```

## update语法
```
1. 修改单表记录
update 表名
set 列='新值',列='新值'....
where 筛选条件

2. 修改多表记录
update 表1 别名
join|left join 表2 别名
on 连接条件
set 列='新值',列='新值'....
where 筛选条件

案例1：把张无忌女朋友手机号码改成114
UPDATE boys a
JOIN beauty b
ON a.id=b.boyfriend_id
SET b.phone='114'
WHERE a.boyname='张无忌'

案例2：把没有男朋友的女神的男朋友编号改成2
UPDATE beauty a 
LEFT JOIN boys b
ON a.`boyfriend_id`=b.`id`
SET a.`boyfriend_id`=7
WHERE b.id IS NULL
```

## delete语法
```
方式一：
1. 单表删除
delete from 表  where 筛选条件

2. 多表删除
delete 表1的别名,表2的别名
from 表1 别名
join|left join 表2 别名
no 连接条件
where 筛选条件

案例1：删除张无忌的女朋友
DELETE a
FROM beauty a
LEFT JOIN boys b
ON a.`boyfriend_id`=b.`id`
WHERE b.boyname='张无忌'

案例1：删除黄晓明及女朋友的信息
DELETE a,b
FROM beauty a
LEFT JOIN boys b
ON a.`boyfriend_id`=b.`id`
WHERE b.boyname='黄晓明'


方式二：
truncate table 表 

delete和truncate比较
1. delete可以加where条件，truncate不能加where条件
2. truncate删除效率更高，属于物理删除，delete属于逻辑删除
3. 表中有自增的列
   delete删除后，再插入数据，自增长列的值从断点开始；truncate删除后，再插入数据，自增列的值从1开始
4. truncate删除没有返回值
5. truncate删除不能回滚，delete删除可以回滚
```