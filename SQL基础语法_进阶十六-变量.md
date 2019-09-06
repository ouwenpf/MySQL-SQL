## 变量

- 系统变量
```
1. 定义：变量由系统提供，不是用户定义，属于服务器层面
2. 分类：
	全局变量：
		作用域：针对所有的会话(连接)有效，但服务不能重启
	会话变量；
		作用域：仅仅针对当前会话连接
3. 使用语法：
	1. 查询所有的系统变量
		show global|[session] variables;
	2. 查看满足条件的部分系统变量
		show global|[session] variables like '%系统变量名%'
	3. select @@global|[session].系统变量名
	4. 为某个系统变量赋值
		set global|[session] 系统变量名= 值
		set global|session.系统变量名= 值
	注意：如果是全局级别，则需要加global；如果是会话级别，则需要加上session，如果不写，默认session
```
- 自定义变量
```
1. 定义：变量是用户自定义，不是系统提供
2. 使用步骤
	声明
	赋值
	使用(查看，比较，运算等)
3. 类别
	用户变量
		作用域：针对当前会话(连接)有效，等同于会话变量的作用域,应用在任何地方，也就是begin end里面或者外面
		声明并初始化：
			set @用户变量名=值
			set @用户变量名:=值
			select @用户变量名:=值
		赋值：
			1. set @用户变量名=值
			2. select 字段 into @变量名
			from 表
		使用：
			select @用户变量名
	局部变量
		作用域：仅仅在定义在begin end中有效，而且只能在begin end中的第一句话
		严格遵守三部曲：
						1. 声明：
								declare 变量名 类型;
								declare 变量 类型 default 值;
						2. 赋值
								set 用户变量名=值
								set 用户变量名:=值
								select @用户变量名:=值
								select 字段 into 变量名
								from 表
						3. 使用
								select 用户变量名
								
								



案例1：用户变量
SET @a=1;
SET @b=2;
SET @sum = @a+@b;
SELECT @sum ;

案例2：局部变量
BEGIN
	DECLARE a INT DEFAULT 1;
	DECLARE b INT DEFAULT 2;
	DECLARE `sum` ;
	SET `sum` = a+b;
	SELECT `sum`;
END
;
```		
