## 库的操作

```
语法：
create database [if not exists] 库名
alter database 库名 character set utf8 cllate utf8_general_ci;
drop database [if exists] 库名

```
## 表的操作
```
1. 表的创建
create table [if not exists] 表名(
	列名 列的类型【长度,约束】
	列名 列的类型【长度,约束】
	...
	列名 列的类型【长度,约束】
);

2. 表的修改
	a.修改列名
	b.修改列的类型或者约束
	c.添加新列
	d.删除列
	f.修改表名

A.修改列名：
alter table 表 change 旧列名 新列名 列的类型【长度,约束】[FIRST|AFTER col_name]

B.修改列的类型或者约束
alter table 表 modify 列名 列的类型【长度,约束】[FIRST|AFTER col_name]

C.添加新列
alter table 表 add 列名 列的类型【长度,约束】[FIRST|AFTER col_name]

D.删除列
alter table 表 drop  列名 
alter table 表 alter 列名 set default '默认值' ;添加默认值
alter table 表 alter 列名 drop default; 删除默认值

F.修改表名
rename table 旧表 to 新表

3.表的复制
create table 新表 like 旧表 ;复制旧表的结构+索引
create table 新表 select * from 旧表 ;只复制旧表的数据，不包含索引 
在gtid环境中不支持此操作，不建议使用此方法

create table 新表 
select 列名1, 列名2, ... from 旧表 where 筛选条件;复制部分数据

create table 新表 
select 列名1, 列名2, ... from 旧表 where 1=2;复制部分结构


```