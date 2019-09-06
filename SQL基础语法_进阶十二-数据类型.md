## 数据类型

- 整形
```
tinyint,int,bigint

特点：
1. 如果不设置无符号还是有符号，默认是有符号，如果想设置无符号，需要添加关键字unsigned
2. 如果插入的数据超过范围，会报out of range异常
3. 如果不设置长度默认的长度int为11，比bigint为20
4. 长度为显示的宽度，如果不够会用0左边填充，但必须搭配zerfill使用
```
- 小数
```
1. 浮点型
	float(M,D)
	double(M,D)
2. 定点型
	dec(M,D)
	decimal(M,D)
	
特点：
M：整数部位+小数部位
D：小数部位

M和D可以省略
如果是float和double，则会根据插入的数字的精度来决定(不超过类型的范围即可插入任何值)
如果是decimal,M默认为10，D默认为0
```
- 字符
```
较短的文本
varchar

枚举
enum 
CREATE TABLE test(
	f1 ENUM('a','b','c')
);
集合
set
CREATE TABLE test(
	f1 SET('a','b','c')
);

较大文本
text
blob

```

- 日期
```
datetime:保存日期+时间
timestamp:保存日期+时间

特点：
			字节			范围				是否受时区影响
datetime	8			1000-9999		   是
timestamp   4		   1970-2038	      否	

```


- 所选择的类型越简单越好，能保存的数值类型越小越好