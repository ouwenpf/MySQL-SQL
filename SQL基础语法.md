#SQL基础语法

- 创建数据库
```
CREATE DATABASE [IF NOT EXISTS] dataname CHARACTER SET utf8 COLLATE utf8_general_ci;
ALTER DATABASE dataname CHARACTER SET utf8 COLLATE utf8_general_ci;
对于不知道数据库默认字符和校对规则的情况下，建议加上CHARACTER SET utf8 COLLATE utf8_general_ci

```
- 表的操作
```
create table tb_name (
	id int [unsigned] [not null] [auto_increment] [comment 'description'],
	uid int [unsigned] [not null] [comment 'description'],
	var varchar(32) [not null] [default '0'] [comment 'description'],
	primary key (id),
	unique key (uid),
	key var (var)
)ENGINE=InnoDB  [AUTO_INCREMENT=1000] DEFAULT CHARSET=utf8 COLLATE=utf8_general_ci [ROW_FORMAT=Dynamic];

create table tb_name like old_tb_name 包括旧表的结构+索引
create table tb_name select *  from old_tb_name 包括旧表的结构+数据
ERROR 1786 (HY000): CREATE TABLE ... SELECT is forbidden when @@GLOBAL.ENFORCE_GTID_CONSISTENCY = 1 在gtid环境中不支持此语法


create table test
 (
 ID int not null COMMENT 'ID值',
 Name varchar(20) not null COMMENT '姓名',
 Age varchar(20) not null COMMENT '年龄',
 Sex varchar(20) not null COMMENT '性别',
 Vocation varchar(20) not null COMMENT '职业',
 CardID bigint not null COMMENT '身份ID信息',
 Joindate datetime not null COMMENT '加入日期',
 Region varchar(12) not null COMMENT '地区',
 Tel varchar(12) not null COMMENT '移动电话号码',
 Email varchar(30) not null COMMENT '电子邮箱',
 Recommend varchar(10) COMMENT '推荐信息',
 Identifier varchar(100) COMMENT '身份识别信息'
 
 ) ;
 

-- 改表名

RENAME TABLE test  TO test1;
ALTER TABLE  test RENAME  TO test1  ;

ANALYZE TABLE 适用于 更新索引的统计信息，优化查询，但它不会影响表的物理结构。
ALTER TABLE test  ENGINE = 'InnoDB' 来替换
OPTIMIZE TABLE 
适用于 回收空间 和 整理碎片，通过重新构建表和索引来优化存储。
在实际使用中，通常建议定期使用 ANALYZE TABLE 来保持查询优化器的准确性，而 OPTIMIZE TABLE 主要在 大量删除或更新数据之后，或者表空间使用不合理时使用。
1.OPTIMIZE TABLE只对MVISAM，BDB和InnoDB表起作用，尤其是MyISAM表的作用最为明显。然而并不是所有表都需要进行碎片整理，一般只需要对包含变长的文本数据类型(varchar)的表进行碎片整理。
2.在OPTIMIZE TABLE运行过程中，MySQL会锁定表,
3.默认情况下，直接对InnoDB引擎的数据表使用OPTIMIZE TABLE，可能会显示[Table does notsupport optimize, doing recreate+analyze instead」的提示信息。需要在mysqld启动mysql的时候加---skip-new或--safe-mode

-- 新增列

ALTER TABLE test  ADD  COLUMN   `Address`  VARCHAR(32) ;
ALTER TABLE test  ADD COLUMN (`Address1` VARCHAR(32)  , `Address2` VARCHAR(32) ) ;


--修改列名称
ALTER TABLE test CHANGE COLUMN `Address1`   `Address3`  VARCHAR(32);


--修改列属性
ALTER TABLE test MODIFY COLUMN     `Address3`  VARCHAR(64);


-- 增加和删除默认值

ALTER TABLE test  ALTER COLUMN  `Address3`  SET DEFAULT '北京';

-- 删除
ALTER TABLE test  DROP COLUMN `Address1`;
ALTER TABLE test  DROP COLUMN `Address2`;
ALTER TABLE test  DROP COLUMN `Address3`;



-- 添加/删除约束
ALTER TABLE test  ADD PRIMARY KEY (`ID`)  ;

ALTER TABLE test ADD UNIQUE KEY|INDEX [index_name] (`CardID`)  ;
ALTER TABLE test ADD CONSTRAINT [index_name]  UNIQUE (`CardID`)  ;  --pg语法

ALTER TABLE test ADD KEY|INDEX [index_name] (`Tel`)  ;
create index [index_name]  on   test  (`Tel`)  ;   --pg语法

ALTER TABLE test DROP PRIMARY KEY ;

ALTER TABLE test DROP KEY `CardID` ;
ALTER TABLE test DROP CONSTRAINT `CardID` ; --pg语法

ALTER TABLE test DROP KEY `Tel` ;
DROP INDEXdex  `Tel` ;  --pg语法






1.修改表名称
rename table old_table_name  to new_table_name;
也适用数据库之间的表移动

2.修改默认字符集和排序规则
ALTER TABLE table_name  CHARACTER SET utf8 collate utf8_general_ci;
仅修改table_name表的默认字符集合排序集，并不会修改已有记录的字符集和排序集
ALTER TABLE table_name  CONVERT TO CHARACTER SET utf8 collate utf8_general_ci;
会修改table_name的默认字符集和排序集，也会修改表中已有记录的字符集和排序集

3.新增列
ALTER TABLE table_name ADD column_name VARCHAR(50) NOT NULL DEFAULT '北京'  [AFTER col_name];
ALTER TABLE table_name ADD (number1 BIGINT,number2 BIGINT);新增多列
注意：默认新增列是追加，可以使用first新增在第一列，也可以使用after 
5
4. 删除列
ALTER TABLE table_name drop column_name;删除列不能同时删除多个

5. 修改列
change修改列名称
CHANGE [COLUMN] old_col_name new_col_name column_definition MODIFY [COLUMN] col_name column_definition [AFTER col_name]
ALTER TABLE table_name CHANGE  old_col_name new_col_name VARCHAR(50) NOT NULL DEFAULT '北京' COMMENT '地址';
修改列表需要原来列表的属性，否则无法修改可以使用show create table 查询一下表结构

6. 修改列属性
modify修改列属性
ALTER [COLUMN] col_name {SET DEFAULT literal | DROP DEFAULT} [ AFTER col_name]
ALTER TABLE table_name modify col_name VARCHAR(50) NOT NULL DEFAULT '北京' COMMENT '地址' AFTER age;
change可以通用，modify不能修改字段名称，相对起来modify使用起来更加的简单


alter增加和删除默认值
ALTER [COLUMN] col_name {SET DEFAULT literal | DROP DEFAULT}
ALTER TABLE table_name  ALTER addr1 SET DEFAULT '北京';
ALTER TABLE test ALTER addr1 DROP DEFAULT;
```

- 索引
```
alter table table_name add primary key (col_name,....);
alter table table_name add unique key [index_name]  (col_name,....);
alter table table_name add key [index_name] (col_name,....);

alter table table_name drop primary key ;
alter table table_name drop key index_name ;

alter table  t4 add foreign key (zid) references t3 (zid);
添加外键，字段类型需要一样，mysql中尽量少用外键 
```

- 运算
```
select 后面跟着需要展示的字段或者自定义别名
from：从哪个结果集中
where：查询条件

算术运算
select 3/2;显示1.5
select 3 div 2整除;显示1
select 3/0 ;显示null

比较运算
SQL中true表示1，false表示0
select 1=1显示1；select 0=1显示0
is null && is not null 
between... and ...  in  not in

```

## select 

```
select 字段|表达式  from 表|视图|结果集，字段，表达式，表，结果集都可以使用别名
where 条件

条件之后...
group by 分组
having 分组之后进行检索
order by 排序
limit 限制结果
字段作为条件最好不要计算如
select * from  tb_name where collumn_name+100>=2000
换成
select * from  tb_name where collumn_name>=2000-100

```
## 通配符

```
like %:%匹配0到任意个字符
like _:_匹配单个任意字符
```

## 函数
```
常用数学函数
select CEIL();向上取整
select FLOOR();向下取整
select ROUND();四舍五入
select MOD();取模
select RAND();随机数[0,1)
select POW();幂函数
select CONV();转换函数如二级制转换16进制

常用字符函数
select LENGTH();获取字符长度
select LOWRE();转换小写
select UPPER();转换大小
select SUBSTR();下标从1开始，截取多个字符
select PEPLACE();替换
select TRIM();去掉两端空格
select LPAD(col_name,100,'a');左填充,100表示整体长度，a表示填充值
select RPAD(col_name,100,'a');右填充

日期函数：
select NOW();当前时间
select SYSDATE();当前系统时间
select LAST_DATE();计算某月的最后一天
select date_add(now(),interval 1 year|month|day);
select unix_timestamp(now());linux时间戳
select datediff(now(),'2016-08-30 00:00:00');

聚合函数：
min(),max(),avg(),sum()
count(*) 总记录数
count(col_name)字段的非NULL记录数

分组函数(一般都是和聚合函数配合使用)
group by 条件只能使用 having


```

## 联合查询

```
内连接：
select * from t1 a inner join t2 b on a.col_name=b.col_name  
select * from t1 inner join t2 using(col_name)
通用列名字段必须一致，去掉重复的字段
特点：关联表中都出现的字段值才会出现在结果集中
	  内连接与连接的顺序无关，没有主从表之分
	  
外连接：
select * from t1 a left join t2 b on a.col_name=b.col_name ;
依次遍历主表记录，与主表记录进行匹配，如果匹配到则连接展示，否则以null填充

```

## 子查询

```
子查询也叫嵌套查询，将一个查询结果当做另外一个查询的条件或者结果集
单行子查询：查询返回的结果只有一条记录
多行子查询：查询返回的结果有多行记录
其它用法：主查询可以把相应的字段传递给子查询，反过来也成立
		  select * from t1  a where a.col_name > (select sav(b.col_name) from t2 b where a.col_name=b.col_name)

=any()相当于in ()  >any()大于最小值  <any()小于最大值
>all() 大于最大值  <all()小于最小值
in:先执行子查询，将结果返回给主查询，主查询继续执行
exists：先执行主查询，将主查询的结果依次在主查询中进行匹配，根据匹配结果反馈true和false；如果是true就显示否则就不显示

```

## 联合查询

```
union all/union
	union去重
	联合结果集必须一致

```

