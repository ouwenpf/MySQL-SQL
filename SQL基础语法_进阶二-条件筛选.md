# 基础查询

## 条件查询
```
select 
		查询列表
from
		表名
where	
		筛选条件
		
分类：
		一、按条件表达式筛选
			简单条件运算符：> < =  <> >= <=
		二、按逻辑表达式筛选
			作用：用于连接条件表达式
			逻辑运算符：and or not
		三、模糊查询
			like:一般和通配符搭配
					%：任意多个字符，包含0个字符
					_：任意单个字符
					其它特殊情况：查询中包含通配符的字符可以使用'\'或者
					SELECT *  FROM  employees WHERE last_name LIKE '_$_%' ESCAPE '$'
			between and：
				1. 使用between and可以提高语句的简洁度
				2. 包含临界值
				3. 两个临界值顺序不能调换
			in：
				1. 使用in可以提高语句的简洁度
				2. in列表值类型一样或者兼容
				3. in不支持通配符，属于等值查询
			is null|is not null
				1. =或<>不能用于判断null值，只有is null或者is not null可以判断
				2. 补充说明安全等于<=>，既可以判断null值又可以判断普通的数值，只是可读性差
			 
```