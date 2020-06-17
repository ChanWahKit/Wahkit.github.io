---
title: MySQL事务
categories:
 - summary
tags:
 - MySQL
---

> 数据库事务transaction正确执行的四个基本要素：原子性、一致性、隔离性、持久性

## 事务的ACID特性
* 原子性[atomic]

整个事务中的所有操作要么全部完成，要么全部不完成，事务进行过程中遇到的错误，会被RollBack到最初的状态，即事务开始前。

* 一致性[correspondence]

事务开始前与结束后，数据库的完整性约束并没有被破坏。

* 隔离性[isolation]

隔离状态下执行事务，如果有两个事务，运行在同一时间，执行相同的功能，事务的隔离性将确保每一事务在系统中都被认为只有当前事务在使用系统，该属性亦称为“串行化”。

* 持久性[durability]

在事务完成以后，该事务对数据库所作的更改便持久地保存在数据库中，不会被回滚。

## 事务的隔离级别
* 脏读

读出无效数据，一个事务读取了另一个事务尚未提交的数据，调整方法**READ_COMMITED**

| 操作顺序 | 事务A | 事务B |
| --- | --- | --- |
| 1 | Begin | Begin |
| 2 | select balance from account where id = 1;(结果为 1) |  |
| 3 |  | update account set balance = 2 where id = 1; |
| 4 | select balance from account where id = 1;(结果为 2) |  |
| 5 |  | RollBack |
如上，事务B修改数据但是没有Commit，并且在事务A读取了数据后RollBack，此时事务A读取到的数据为脏数据。

* 不可重复读

同一事务，两个相同的查询返回不同的结果，调整方法**REPEATABLE_READ**

| 操作顺序 | 事务A | 事务B |
| --- | --- | --- |
| 1 | Begin | Begin |
| 2 |  | update account set balance = 2 where id = 1; |
| 3 | select balance from account where id = 1;(结果为 1) |  |
| 4 |  | Commit |
| 5 | select balance from account where id = 1;(结果为 2) |  |
如上，事务B修改数据但是没有Commit，并且在事务A读取了数据后Commit，此时事务A再次进行同样读取，前后结果不一致。

* 幻读

与不可重复读类似，但是事务B仅做插入或删除操作，调整方法**SERIALIZABLE_READ**

| 操作顺序 | 事务A | 事务B |
| --- | --- | --- |
| 1 | Begin | Begin |
| 2 | select * from account where balance = 1;(结果为0) |  |
| 3 |  | insert into account(id, balance) value('3','1'); |
| 4 |  | Commit |
| 5 | select * from account where balance = 1;(结果为0) |  |

