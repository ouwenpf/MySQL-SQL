## 视图

- 定义
```
从MySQL5.0.1版本开始提供视图功能，一种虚拟存在的表，行和列的数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成，保存了SQL逻辑，不保存查询结果
```
- 应用场景
```
1. 多个地方用到同样的查询结果
2. 该查询结果使用的sql语句比较复杂

```
- 视图的好处
```
1. 重用sql语句
2. 简化复杂的sql操作，不必知道查询细节
3. 保护数据，提高安全性

```

- 视图操作
```
创建和修改：
	create or replace view view_name
	as
	查询语句
	;
删除：
	drop wiew view_name1,view_name2 ...

查看：show create view_name1;

视图更新：
	一般很少使用，视图中一般包含复杂的sql语句
	具备以下特点的视图不允许更新：
		1. 分组函数,distinct,group by,having ,union或者union all
		2. select包含子查询
		3. 常量视图
		4. join
		5. from一个不能更新的视图
		6. where字句的子查询引用了from子句中的表

```


