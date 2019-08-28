# 基础查询

## select
```
select 	查询列表  from 表名;
1. 查询列表可以是：表中的字段，常量值，表达式，函数
2. 查询列表是一个虚拟的表格


1. 查询表字段
select * from tb_name;

2. 查询常量
select 100;
select 'jonh'

3. 查询表达式
select 100%98

4. 查询函数
select version();


```

## 别名

```
1. 便于理解，更有意义
使用方式： as或者空格
SELECT VERSION() as 版本; 
SELECT VERSION()  版本; 
特殊情况：如果别名有空格或者关键字使用单引号或者双引号
SELECT VERSION() 'out put'; 

```

## 去重

```
使用关键字distinct

select distinct col_name from tb_name;
```
## +号的作用

```
mysql中的+号仅仅只有一个功能：运算符
select 100+90;两个操作数都是数值型，则做加法运算
select '123'+90;只要其中一方为字符型，试图将字符型值转换成数值型；如果转换成功，则继续做加法运算；如果转换失败，则将字符型数值转换成0
select 'john'+90
select null+10;只要其中一方为null，则结果肯定为null

要实现连接字符串请使用concat函数
select concat(col_name1,col_name2),如果有为null的字段可以使用
函数ifnull(commission_pct,0)
函数isnull(commission_pct)为null返回1，否则返回0
```