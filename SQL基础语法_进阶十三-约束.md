## 约束

- 常见约束
```
约束：一种限制，用于限制表中的数据，为了保证表中的数据的一致性(准确性和可靠性)

分类：六大约束
1. primary key：主键约束，用于保证该字段值具有唯一性，并且非空
2. unique：唯一约束，用于保证该字段值具有唯一性，可以为空
3. not null：非空约束，用于保证该字段值不能为空
4. default：默认约束，用于保证该字段有默认值
5. check：检查约束[MySQL中不支持]
6. foreign key：外键约束，用于限制两个表的关系，用于保证该字段的值必须来自于主表的关联列的值

添加约束的时机：
	1. 创建表时
	2. 修改表时

约束的添加分类：
	列级约束：
			六大约束语法上都支持，但是外键约束没有效果
	表级约束：
			除了非空，默认，其它的都支持

```

- 添加列级约束
```
create table tb_name (
	id int unsigned auto_increment primary key comment '主键',
	uid int unsigned not null unique comment '唯一约束',
	seat INT ,
	UName varchar(32) not null comment '非空约束',
	gender char(1) , -- check(gender='男' or gender='女') comment 'check约束',
	age int default 18 comment '默认约束'
);

```

- 添加表级约束
```
CREATE TABLE tb_name1 (
	id INT UNSIGNED AUTO_INCREMENT,
	uid INT UNSIGNED NOT NULL,
	UName VARCHAR(32) NOT NULL,
	seatID INT ,
	gender CHAR(1),
	age INT DEFAULT 18 ,
	PRIMARY KEY(id),
	UNIQUE KEY(uid),
	KEY(seatID),
	CONSTRAINT fk_seatID_id FOREIGN KEY(seatID) REFERENCES tb_name2(id) 
); 


```
- 主键和唯一索引区别
```
		是否保证唯一性		是否允许为空		是否能有多个		是否允许组合
主键		是					否				否				是
唯一		是					是				是				是

外键：
	1. 要求在从表中设置外键
	2. 从表中的外键列的类型和主表关联列的类型必须一致或者兼容，列名无要求
	3. 主表的关联列必须是一个key(主键，唯一，普通，外键均可)
	4. 插入数据时，先插入主表，再插入从表；删除数据时候，先删除从表，在删除主表
```

- 约束相关操作
```
主键约束
alter table tb_name add primary key(id);
alter table tb_name drop primary key;

唯一约束
alter table tb_name add unique key(uid);
alter table tb_name drop key key_name;

普通约束
alter table tb_name add  key(uid);
alter table tb_name drop key key_name;

外键约束
alter table tb_name add  constraint fk_seatid_id foreign key(seatid) references tb_name2(id);
alter table tb_name drop foreign key fk_seatid_id;
--级联删除
alter table tb_name add  constraint fk_seatid_id foreign key(seatid) references tb_name2(id) on delete cascade;
--级联置空
alter table tb_name add  constraint fk_seatid_id foreign key(seatid) references tb_name2(id) on delete set null;


非空约束
alter table tb_name modify uid int not null;
alter table tb_name modify uid int ;

默认约束
alter table tb_name modify age int default '18';
alter table tb_name modify age int ;

```

- 标识列
```
标识列又称自增长列，可以不用手动插入值，系统提供默认的序列值
特点：
	1. 标识列一般和主键搭配，但是列必须是一个key
	2. 一个表至多只有一个标识列
	3. 标识列可以通过系统参数auto_increment_increment设置，起始列可以手动插入一个值如(1000)或者AUTO_INCREMENT=1000设置(auto_increment_offset在MySQL中不起作用)

```