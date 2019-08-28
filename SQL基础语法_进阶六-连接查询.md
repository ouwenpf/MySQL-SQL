# 基础查询

## 连接查询
```
含义：又称多表查询，当查询的字段来自于多个表时，就会用到连接查询
笛卡尔积现象：表1有m行，表2有n行，结果m*n行
分类：
	按年代分类：
			sql92标准，可以忽略
			sql99标准，推荐使用
	按功能分类：
			内连接：
					等值连接
					非等值连接
					自连接
			外连接：	
					左外连接
					右外连接
					全外连接(mysql目前不支持)
			交叉连接
			
查询连接一般都要为表起别名：
1. 提高语句的简洁度
2. 区分多个重名的字段
注意：如果为表起别名，则查询字段就不能使用原来的表名去限定

语法：
	select 查询列表
	from 表1 别名 [连接类型] 
	join 表2 别名
	on 连接条件
	[where 筛选条件]
	[group by 分组]
	[having 筛选条件]
	[order by 排序列表]

连接类型：
	内连接：inner 
	外连接：
			左外：left
			右外：right
			全外：full
	交叉连接：cross
	
```

## 等值查询
```
#案例1. 查询员工名，部门名
SELECT a.last_name,b.department_name 
FROM employees a 
INNER JOIN departments b 
ON a.`department_id`=b.`department_id`
;

#案例2. 查询员工名字中包含e的员工和工种名称
SELECT last_name,job_title 
FROM employees a 
INNER JOIN 
jobs b
ON a.`job_id`=b.job_id
WHERE last_name LIKE '%e%'
;

#案例3. 查询部门个数>3的城市和部门个数
SELECT city,COUNT(*)
FROM locations a
INNER JOIN
departments b
ON 
a.`location_id`=b.`location_id`
GROUP BY  city
HAVING COUNT(*)>3
;

#案例4. 查询部门的员工个数>3的部门名和员工个数，并按个数降序
SELECT department_name,COUNT(1) AS 员工人数
FROM departments a
INNER JOIN 
employees b
ON a.`department_id`=b.`department_id`
GROUP BY department_name
ORDER BY 员工人数 DESC
;

#案例5. 查询部门名，员工名，工种名
SELECT department_name,last_name,job_title
FROM departments a
INNER JOIN employees b ON a.`department_id`=b.`department_id`
INNER JOIN jobs c ON b.`job_id`=c.`job_id`
;
```

## 非等等值查询
```
案例1：查询员工的工资级别
SELECT last_name,salary,`grade_level` 
FROM employees a
INNER JOIN job_grades b ON 
a.`salary` BETWEEN b.`lowest_sal` AND b.`highest_sal`

案例1：查询工资级别个数，按数量降序排列
SELECT `grade_level`,COUNT(*)
FROM employees a
INNER JOIN job_grades b ON 
a.`salary` BETWEEN b.`lowest_sal` AND b.`highest_sal`
GROUP BY b.grade_level
HAVING COUNT(*) > 20
ORDER BY COUNT(*)  DESC

```

## 自连接
```
经典案例：查询员工名字和上级名字
SELECT a.last_name,b.last_name
FROM employees a
JOIN employees b
ON a.`manager_id` = b.`employee_id`
```


## 外连接
```
应用场景：用于查询一个表中有，另一个表中没有的记录
特点：
	1. 外连接查询结果为主表中所有的记录
			如果从表中有和它匹配的，则显示匹配的值
			如果从表中没有和它匹配的，则显示NULL
			外连接查询结果=内连接+主表中有而从表中没有的记录
	2. 左外连接,left join左边的是主表
	     右外连接,right join右边是主表
	3. 左外和右外交换两个表的顺序，可以实现同样的效果
	
	
案例1. 查询没有员工的部门
SELECT department_name
FROM departments a
LEFT JOIN employees b
ON a.`department_id`=b.`department_id`
WHERE b.employee_id
IS NULL 
;

```