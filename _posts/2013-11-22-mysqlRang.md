---
layout: post
category:mysql
title: 表分区
tagline: by anping
tags: [表分区]
---


mysql 表分区
------------


##我在哪里用到过表分区?

在和工作室和队员以及艾老师设计江西农大学工管理系统时，对数据库设计做了深入的分析。

其中就遇到一个需求就是：学生毕业一年之后就不允许登陆本系统，但是依旧需要保存学生的信息于物理设备中。

如果只是禁止登陆的话并不删除的话，学生信息表会越来越到，甚至隔几年会达到几十万的数量，如果在几十万条的数据中去检索学生信息的话，速度会十分的慢。


##想法
那么我们想到一种方法，是将学生信息表以五年一次做一次分区，并保证在校的学生一定是在同一个区中。

这样，不但提升了检索学生信息的效率，又方便了对毕业学生信息的管理。



##[mysql分区类型](http://baike.baidu.com/view/4318131.htm?fr=aladdin)

从百度百科中明白分区类型有四种。

那么我是这样对学生信息表做分区的。

1.	我选择了使用range分区类型。
2.	然后将入学时间做了分析，因为入学时间是每年的9,10号，则我只需要判断年份就可以知道分到哪个区去
3.	最好以入学时间做分区



	CREATE TABLE student (  
		studentNo varchar(8) primary key NOT NULL,
	   	......
		createTime DATE NOT NULL 
		...... 
	)  
	  
	partition BY RANGE(year(createTime)) 
	(
		partition p0 VALUES LESS THAN ('2014'),  
		partition p1 VALUES LESS THAN ('2018'),  
		partition p2 VALUES LESS THAN ('2022'),  
		partition p3 VALUES LESS THAN ('2026') ,
		partition p4 VALUES LESS THAN MAXVALUE
	);





