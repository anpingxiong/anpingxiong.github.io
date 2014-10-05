---
layout: post
category: mysql
title: 索引
tagline: by anping
tags: [index]
---


mysql 索引
----------


索引是数据库中的重点，因为索引会将查询运行的更快。



索引类型分类
------------

1.	普通索引
2.	唯一索引
3.	主键
4.	组合索引



#普通索引

数据库使用[B+树](http://baike.baidu.com/view/1168762.htm?fr=aladdin)来存储该索引

当一个普通的字段没有添加任何索引时，mysql对它的搜索是顺序搜索的，当数据量很大时，每次查询都需要去查询n遍，大大的较低了查询效率和吞吐量。那么这个字段便可以添加普通索引来加快搜素的速度。

	
	create index indexName on table(columnName);// 第一种创建普通索引的方式
	alter table  tableName add index(columnName);//第二种创建普通索引的方式


##唯一索引

唯一索引和普通索引的不同点就是该索引指向的字段不能够有重复的值。

	
	create  unique	index indexName on table(columnName);



##主键

主键是创建表格时为了维护表的完整性而必须的键.主键的特点是唯一的并且存储在物理是有序的。
	

	primary key 



##组合索引

当在一个表中创建多个普通索引时，可以考虑使用组合索引，因为使用多个普通索引查询的速度比组合索引的查询速度要慢的多。

组合索引需要注意的:
组合索引有一个最左前缀规则，即如果我创建了组合索引(username,age,sex)则相当于创建了如下:

1.	username,age,sex
2.	username,age
3.	username



	create index indexName on  table(column1,column2);




索引的物理分类
--------------

1.	[非聚集索引](http://baike.baidu.com/view/692530.htm?fr=aladdin)
2.	[聚集索引](http://baike.baidu.com/view/687673.htm)


##缺点:
mysql的索引并不是没有缺点的

1.	索引占用物理空间大
2.	索引导致数据更新慢
3.	创建索引会随着索引的量增大而速度降低


##以下语句不会使用到索引
应该避免使用的sql [语句来自网络]

1.WHERE字句的查询条件里有不等于号


	（

		WHERE column!=...

	）

 

 2.类似地，如果WHERE字句的查询条件里使用了函数（如：

	
	WHERE DAY(column)=... 



3.在JOIN操作中（需要从多个数据表提取数据时）


只有在主键和外键的数据类型相同时才能使用索引，否则即使建立了索引也不会使用

 

 

4.如果WHERE子句的查询条件里使用了比较操作符LIKE和REGEXP，MYSQL只有在搜索模板的第一个字符不是通配符的情况下才能使用索引。


比如说，如果查询条件是

	LIKE 'abc%'	 #MYSQL	将使用索引；


如果条件是

	LIKE '%abc' #MYSQL将不使用索引。

 

 

5.在ORDER BY操作中，MYSQL只有在排序条件不是一个查询条件表达式的情况下才使
用索引。
尽管如此，在涉及多个数据表的查询里，即使有索引可用，那些索引在加快
ORDER 	BY操作方面也没什么作用。

 

 

6.如果你在索引列使用函数调用或者更复杂的算术表达式，MySQL就不会使用索引，因为它必须计算出每个数据行的表达式值。
	

	WHERE mycol < 4 / 2 使用索引
	WHERE mycol * 2 < 4 没有使用索引

 

 

 

  

