# 基础查询

## limit语法
```
limit [offset],size;
      offset:要显示条目的起始索引(起始索引从0开始)
      size:要显示的条目个数
特点：
	1. limit语句放到查询语句的最后
	2. 公式，
	     要显示页数page，每页的条目数size
	   limit (page-1)*size,size
```

## 查询语句中执行先后顺序
```
select 查询列表		7           
from 表				1
连接类型 join 表2	2
on 连接条件			3
where 筛选条件		4
group by 分组列表	5
having 分组后的筛选	6
order by 排序列表	8
limit 偏移，条目数	9

```