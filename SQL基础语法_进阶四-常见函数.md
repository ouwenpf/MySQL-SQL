# 基础查询

## 常见函数
```
概念：类似于java方法，将一组逻辑语句封装在方法体中，对外暴露方法名
好处：隐藏了实现细节，提高代码的重用性
调用：select 函数名(实参列表) 
	  select 函数名(实参列表)  from tb_name
特点：
	1. 叫什么(函数名)
	2. 干什么(函数功能)
分类：
	1. 单行函数
	2. 分组函数：做统计使用，又称为统计函数，聚合函数，组函数		 
```

## 单行函数
- 字符函数
- 数学函数
- 日期函数
- 其它函数
- 流程控制函数

### 字符函数
```
length()函数：获取参数值的字节的个数
concat()函数：拼接字符串
upper()|lower()函数:大小写转换
substr():函数
		索引从1开始
		substr(str,1)截取从指定索引 出后面所有的字符
		substr(str,1,3)截取从指定索引处指定字符长度的字符
instr()函数：返回子串第一次出现的索引，如果找不到返回0
trim()函数：
		trim(' str ')去前后空格
		trim('fd' FROM 'fdddfd')把'fd'当做一个整体去掉字符串'fdddfd'前后的'fd',最后返回结果为'dd'

lpad()|rpad()函数：用指定的字符实现左右填充指定的长度
		lpad(str,len,padstr),str处理的对象，len指定长度，padstr填充的字符
replace()函数：替换

```

### 数学函数
```
round():四舍五入
			1. round(1.54)返回2
			2. round(1.545,2)返回1.55
clil()函数：向上取整，返回>=该参数的最小整数
floor()函数：向下取整，返回<=该参数的最大整数
mod()函数：取模
			mod(a,b):a-a/b*b
select RAND();随机数[0,1)
select POW();幂函数
select CONV();转换函数如二级制转换16进制	
truncate()截断；
			truncate(1.56,1)返回1.5		
```

### 日期函数
```
NOW();当前时间
SYSDATE();当前系统时间
LAST_DAY();计算某月的最后一天
date_add(now(),interval 1 year|month|day);
unix_timestamp(now());linux时间戳
datediff(now(),'2016-08-30 00:00:00');
str_to_date：将日期格式字符转换成指定的日期格式
					str_to_date('8-10-2019','%m-%d-%Y'); 返回：2019-08-10
					select str_to_date('2019-08-10','%Y-%m-%d'); 返回：2019-08-10 
date_format：将日期转换成字符
					date_format(now(),'%Y年%m月%d日')返回：2019年08月10日 
					select date_format(DATE_ADD(NOW(), INTERVAL 2 DAY), '%Y-%m-%d %T');
```

### 流程控制函数
```
1. IF(expr1,expr2,expr3)
条件表达式expr1成立，返回expr2，否则返回expr3
案例：
SELECT last_name,commission_pct,IF(commission_pct IS NULL,'呵呵','嘻嘻')
FROM employees;


2. case (等值判断)
语法：
		case 变量|表达式|字段
		when 要判断的值 then 返回的值1
		when 要判断的值 then 返回的值2
		...
		else 返回的值n
		end
		注意：update中使用case，else最好带上(变量|表达式|字段)，不然全部更新，需要特别注意
		
案例：查询员工的工资，要求
			部门编号=30，显示的工资为1.1倍
			部门编号=40，显示的工资为1.2倍
			部门编号=50，显示的工资为1.3倍
			其它部分，显示的工资为原始工资
SELECT department_id,salary,
CASE department_id
WHEN 30 THEN salary*1.1
WHEN 40 THEN salary*1.2
WHEN 50 THEN salary*1.3
ELSE salary 
END AS new_salary
FROM employees ORDER BY department_id;

3. case (条件判断)
语法：
		case 
		when 要判断的条件1 then 返回的值1
		when 要判断的条件2 then 返回的值2
		...
		else 返回的值n
		end
		注意：update中使用case，else最好带上(变量|表达式|字段)，不然全部更新，需要特别注意
		
案例：查询员工的工资的情况
如果工资>20000,显示A级别
如果工资>15000,显示B级别
如果工资>10000,显示C级别
否则，显示D级别

select salary,
case
when salary>20000 then 'A'
when salary>15000 then 'B'
when salary>10000 then 'C'
else 'D'
end as 级别
from employees;
```

## 分组函数 
```
sum(),agv(),max(),min(),count()
特点：参数支持哪些类型
1. sum(),agv()只支持数值类型
   max(),min(),count()可以处理任何类型
2. 以上分组函数都忽略null值
3. 可以和distinct搭配实现去重运算
4. 一般使用count(*)或者count(1)用作统计行数
5. 和分组函数一同查询的字段要求是group by后的字段

```

