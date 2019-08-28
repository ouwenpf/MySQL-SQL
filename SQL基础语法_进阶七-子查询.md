# 基础查询

## 子查询
```
含义：出现在其它语句内部的(select,insert,update,delete)语句，称为子查询或内查询

分类：
查询子查询查询的位置
		 select后面
		 		仅仅支持标量子查询
		 from后面
		 		支持表子查询
		 where或having后面
		 		标量子查询
		 		列子查询
		 		行子查询
		 exists后面
		 		表子查询
按结果集的行列数不同
	标量子查询(结果集只有一行一列)
	列子查询(结果集只有一列多行)
	行子查询(结果集有一行多列)
	表子查询(结果集一般为多行多列)
```

### where或者having后面
```
1. 标量子查询(单行子查询)
2. 列子查询(多行子查询)
3. 行子查询(多行多列)

特点：
1. 子查询放在小括号内
2. 子查询一般放在条件的右侧
3. 标量子查询，一般搭配着单行操作符使用
   > < = >= <= <>
4. 子查询优先于主查询

列子查询，一般搭配着多行操作符使用
in/not in,any,all

 
```

###标量子查询
```
案例1：谁的工资比Abel高
SELECT *
FROM  employees
WHERE salary>(
	SELECT salary 
	FROM `employees`
	WHERE last_name='Abel'
)

案例2：查询和141员工相同的job_id,salary比143号员工多的last_name,salary,job_id
SELECT last_name,salary
FROM employees
WHERE job_id=(
	SELECT job_id
	FROM employees 
	WHERE employee_id=141
) AND salary >(
	SELECT salary 
	FROM employees
	WHERE employee_id=143
)
;


案例3. 返回工资最少的员工的last_name,job_id和salary
SELECT last_name,job_id,salary
FROM employees
WHERE salary=(
	SELECT MIN(salary)
	FROM employees 
)
;


案例4：最低工资大于50的id，查询其部门和最低工资
SELECT department_id,MIN(salary)
FROM employees
GROUP BY department_id
HAVING MIN(salary)>(
	SELECT MIN(salary)
	FROM employees
	WHERE department_id=50 
) AND department_id IS  NOT NULL 
;
```

### 列子查询
```
案例1：查询location_id 是1400或1700的部门中所有员工
SELECT last_name
FROM employees
WHERE department_id 
IN (
	SELECT department_id  
	FROM departments
	WHERE location_id  IN(1400,1700)
)
;


案例2：比job_id为IT_PROG员工工资都低的其它部分员工的id，名字，工资和工种
SELECT employee_id,last_name,salary,job_id
FROM employees
WHERE salary<ALL(
	SELECT salary
	FROM employees
	WHERE job_id='IT_PROG'
) AND job_id<>'IT_PROG'
;
等价于
SELECT employee_id,last_name,salary,job_id
FROM employees
WHERE salary <(
	SELECT MIN(salary)
	FROM employees
	WHERE job_id='IT_PROG'	
) AND job_id<>'IT_PROG'
;


案例3：行子查询，用得较少

SELECT * 
FROM employees
WHERE (employee_id,salary)=(
	SELECT employee_id,salary
	FROM employees
	WHERE employee_id=100

)
```


### select后面子查询(仅仅支持标量子查询)
```
查询每个部门的员工数，部门没有的员工数为0
SELECT a.*,(
	SELECT COUNT(*)
	FROM employees b
	WHERE a.`department_id`=b.`department_id`

) 人数
FROM departments a
注意：此种写法相对于聚合函数统计更加好，可以统计不存在的为空，适合如部门多少人，部门表字段全部列出

SELECT a.*,(
	SELECT COUNT(*)
	FROM employees b
	WHERE b.`job_id`=a.`job_id`
) 人数
FROM jobs a
ORDER BY 人数 DESC

SELECT (
	SELECT department_name 
	FROM departments a
	JOIN employees b
	ON a.`department_id`=b.`department_id`
	WHERE b.`employee_id`=102
) 部门
;
```
### from后面子查询
```
将子查询结果集充当一张表，要求必须起别名

案例：查询部门的平均工资的工资等级
SELECT t1.department_name,t1.salary,j.`grade_level` 
FROM 
(
	SELECT a.department_name,AVG(salary) salary
	FROM departments a
	LEFT JOIN employees b
	ON a.`department_id`=b.`department_id`
	GROUP BY a.`department_name` 
	HAVING AVG(salary) IS NOT NULL 
)t1
JOIN job_grades j
ON t1.salary BETWEEN j.`lowest_sal` AND j.`highest_sal`
ORDER BY j.grade_level

如果没有工资等级表
SELECT *,
	CASE 
	WHEN salary>20000 THEN 'A'
	WHEN salary>15000 THEN 'B'
	WHEN salary>10000 THEN 'C'
	WHEN salary>5000 THEN 'D'
	ELSE 'E'
	END AS 等级
FROM
(
	SELECT a.department_name,AVG(salary) salary
	FROM departments a
	LEFT JOIN employees b
	ON a.`department_id`=b.`department_id`
	GROUP BY a.`department_name` 
	HAVING AVG(salary) IS NOT NULL 
)t1

```

### exists后面子查询
```
案例1：查询有员工的部门

SELECT department_name 
FROM departments a
WHERE EXISTS (
	SELECT a.department_id
	FROM employees b
	WHERE a.department_id = b.department_id 
)

SELECT department_name 
FROM departments
WHERE department_id IN(
	SELECT department_id 
	FROM employees
)

案例2：查询有女朋友的男神信息
SELECT *
FROM boys a
WHERE  EXISTS (
	SELECT b.boyfriend_id
	FROM beauty b
	WHERE a.id = b.boyfriend_id	
)

SELECT * 
FROM boys a
WHERE id IN (
	SELECT b.boyfriend_id
	FROM beauty b
)

注意：子查询中exists都可以使用in
```

