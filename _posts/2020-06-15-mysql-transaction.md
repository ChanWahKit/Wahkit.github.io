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

