# 基础查询

## 分组查询
```
语法：
			select 分组函数，列(要求出现在group by 的后面)
			from 表
			[where 筛选条件]
			group by 分组的列表
			[order by字句]
注意：
			查询列表必须特殊，要求是分组函数和group by后面的字段

特点：
			1. 分组查询中的筛选条件分为两类
			
							    数据源						位置						关键字
			分组前筛选			原始表						group by字句的后面		   where
			分组后筛选			分组后的结果集		    	 group by字句的后面			having
			1. 分组函数做条件肯定是放在having字句中
			2. 能用分组前筛选的，就优先考虑使用分组前筛选
			3. group by字句支持单个字段分组，多个字段分组(多个字段直接用逗号隔开没有顺序要求)，表达式或者函数(用得比较少)
			4. 也可以添加排序(排序放在整个分组查询的最后)
```

### 案例
```
#案例1. 查询每个工作的最高工资
select max(salary),job_id from employees group by job_id ;

#案例2. 查询每个位置的部门的数量
SELECT location_id,COUNT(*) FROM departments GROUP BY location_id ;

#.添加分组前筛选条件
案例1：查询邮箱中包含a字符的，每个部门的平均工资
SELECT department_id,AVG(salary) FROM employees 
WHERE email LIKE '%e%'
GROUP BY department_id ;

案例2：查询有奖金的每个领导手下的员工最高工资
SELECT manager_id,MAX(salary) FROM employees
WHERE commission_pct IS NOT NULL 
GROUP BY manager_id;

#添加分组后的筛选条件
案例1：查询哪个部门的员工个数>2
select department_id ,count(1) as `count` from employees
group by department_id 
having `count`>2;
根据分组结果集再进行筛选，需要使用having(在原始数据中筛选使用where)

案例2：查询每个工种的奖金的员工的最高工资>12000的工种编号和最高工资
SELECT job_id,MAX(salary) AS max_salary FROM employees
WHERE commission_pct IS NOT NULL
GROUP BY job_id 
HAVING max_salary>12000

案例3：查询领导编号大于102的，此领导下面员工最低工资>5000的领导编号和员工工资
SELECT manager_id ,MIN(salary) AS min_salary FROM employees
WHERE manager_id>102
GROUP BY manager_id
HAVING  min_salary>5000
;

#按表达式或函数分组
案例1：按员工姓名的长度分组，查询每一组的员工的个数，筛选员工的个数>5的有哪些
SELECT LENGTH(last_name) AS last_name ,COUNT(1) FROM  employees
GROUP BY LENGTH(last_name) 
HAVING COUNT(1)>5
;

#按多个字段分组
案例1：查询每个部门的每个工种的员工平均工资
SELECT department_id,job_id,AVG(salary) FROM employees
GROUP BY department_id,job_id
;

#分组后进行排序,后面可以使用order by等字句
SELECT department_id,job_id,AVG(salary) FROM employees
WHERE department_id IS NOT NULL
GROUP BY department_id,job_id
HAVING AVG(salary)>10000
ORDER BY AVG(salary) DESC
;
```