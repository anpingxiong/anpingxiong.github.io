---
layout: post
category: mysql
title:  事务 
tagline: by anping
tags: [transaction]
---

mysql事务 
---------

##锁机制
1.	共享锁
2, 	排他锁
##锁粒度
1.	行锁
2.	表锁
3.  库锁


##mysql 事务的特性

1.	原子性(每一个事务都是最小并不可划分出其它的事务)
2.	一致性(简单的来说是保证数据库的完整性,比如在银行转账中，A向B转账，A减了则B也应该也要做出相应的改变)
3.	隔离性(每一个事务都是单独运行，互不干扰)
4.	持久性(每一个事务对数据库数据做了更新都应该保证在物理存储设备中)


##异常
当在多个事务同时工作时会发生数据如下几种坏情况:
1.	数据读脏
比如事务A,对数据库中某数据做了修改，但是还没有提交事务,当事务B去读取的时候，却读到了事务A更新的数据.

2.	不可重复读

比如事务A在读取数据时事务还没有结束，假设读取的结果是a, 然后在此时，数据又被事务B修改成C,而事务A还没有结束，继续读取此数据时却是c,则出现了在同一个事务中读取同一个数据不相同的情况。

3.	幻数

是指不同的事务对同一个数据修改时，发生修改之后的数据和事务需要预期的不一致。



##事务隔离级别

那么在处理以上情况则可以选择不同的事务隔离级别:

1.不提交读(read uncommitted)[脏数据]




	set session transaction isolation level read uncommitted;//sql
	
	connection.setTransaction(Connection.TRANSACTION_READ_UNCOMMITTED);//java



2.提交读(read committed)[避免脏数据]
	
	

	set session  transaction isolation level read committed;//sql

	
	connection.setTransaction(Connection.TRANSACTION_READ_COMMITTED);//java




3.重复读(repeatable read)[mysql默认事务,解决不可重复读]

	
	set session  transaction  isolation level repeatable read;//sql

	
	connection.setTransaction(Connection.TRANSACTION_REPEATABLE_READ);//java



4.序列化(serializable)[象加了共享锁]




	set session transaction isolation level  serializable;//sql


	connection.setTransaction(Connection.TRANSACTION_SERIALIZABLE);//java




##不提交读 sql 测试
会导致 读脏数据(如下，在事务B未提交的情况下，事务A读取到了B更新的数据)
比如:



	//事务A 
	set session  transaction isolation  level  read uncommitted;
	//事务B
	set autocommit=off;
	set	session transaction  isolation  level  repeatable read;#系统默认
    insert into t_anping values(1,'anping');	

	//此时事务B并没有去提交事务
	//事务A去查询t_anping 
	//事务A
	select  * from t_anping ; #查询到事务B 插入的数据









