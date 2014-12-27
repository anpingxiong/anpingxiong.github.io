---
layout: post
category: mysql
title: mysql 集群 masterAndSlave
tagline: by anping
tags: [mysql,master slave,数据库集群]
---

mysql 集群
-----------

在大三的时候便对数据库集群有所了解,并且自己曾下载安装过两个数据库试图搭建数据库集群但并不如意，并且对master/slave的架构并不清楚。
今天便记录对master/slave 架构， mysql 是如何做到主从数据库数据一致的。


##mysql 主从复制的方式


*   基于sql复制
*   基于行复制
*   sql与行复制混合

##mysql 复制是如何工作的

*   首先master 接受了更新事务(增删改查),则或将该事务写入到二进制日志中.
*   slave数据库服务器则启动着两个线程:


*   一个是线程是负责通过IO的方式讲二进制中的刚加入的更新日志读取到slave的中继日志中.
*   第二个线程是Sql线程，将事务从日志中读取出来并在数据库中执行



以上是自己通过阅读博客了解并总结的知识，博客地址如下:


[高性能Mysql主从架构的复制原理及配置详解 ](http://blog.csdn.net/hguisu/article/details/7325124)


##公司中如何方便的使用maste/slave　模式的mysql集群呢？

*   使用rose框架,简单的只是需要在@SQL　注解上添加@Slave　　　[rose](http://www.54chen.com/life/rose-manual-1.html)
*   与rose搭配使用的zookeeper，zookeeper中配置了集群数据库分别所在的host与端口，以及主从策略


##我从公司构建分支式的架构中学到了什么?



*   首先需要一个像zookeeper一样的协调系统，用来管理分布式系统中的所有配置
*   可以针对zookeeper中定义配置规则来写对于的插件或者框架，比如方便的@slave注解，其实内部需要去获取zookeeper中需要slave服务器的配置。再做操作。或则对rabiitmq做分布式封装。
*   需要强大的运维人员(自己也需要对这方面知识加强学习)
