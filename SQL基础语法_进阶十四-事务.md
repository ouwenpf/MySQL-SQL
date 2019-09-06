## 事务

- 事务的属性
```
1. 原子性：事务是一个不肯分割的工作单位，事务中的操作要么都发生，要么都不发生
2. 一致性：事务必须使数据库从一个一致性状态换到另外一个一致性的状态
3. 隔离性：一个事务的执行不能被其它事务干扰，即一个事务内部的操作及使用的数据对并发的其它事务是隔离的，并发执行的各个事务之间不能相互干扰
4. 持久性：一个事务一旦被提交，它对数据库中的数据改变是永久性的，接下来的其它操作和数据库故障不应该对其有任何影响

```
- 事务创建
```
begin;
DML语句;
DML语句;
....
commit|rollback;

```
- 事务隔离性
```
对于同时运行的多个事务，当这些事务访问数据库中相同的数据时，如果没有采取必要的隔离机制，就会导致各种并发的问题
脏读：对于两个事务T1,T2,T1读取了已经被T2更新但是还没有被提交的字段，若T2回滚，T1读取的内容就是临时且无效
不可重复读：对于两个事务T1,T2,T1读取了一个字段，然后T2更新了该字段，T1再次读取同一个字段，值就不同了
幻读：对于两个事务T1,,T2,T1从一个表中读取了一个字段，然后T2在改表中插入了一些新的行，如果T1再次读取同一个表，就会多出几行
```
- 事务的隔离级别
```
READ-UNCOMMITED(都未提交)
READ-COMMITED(都已提交)
REPEATABLE-READ(可重复读)
SEARALIZABLE(串行化)
set session transaction isolation level repeatable read;
set session transaction_isolation='repeatable-read';

```

- 事务回滚点
```
begin;
DML语句;
savepoint name1;
DML语句;
savepoint name2
....
rollback  to name1;


```