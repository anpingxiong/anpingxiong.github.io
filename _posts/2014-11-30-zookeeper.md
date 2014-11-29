---
layout: post
category: zookeeper
title: zookeeper协调系统总结
tagline: by anping
tags: [zookeeper]
---


zookeeper
---------

zookeeper是什么？  <br>

zookeeper初接触的时候是在刚接触公司代码时，刚开始对zookeeper不是很理解，只是知道zookeeper里是存放了系统的很多配置，这样可以用来方便的管理
分布式系统。但是实际上zookeeper的功能不单单是用来存放配置文件。

*   统一命名服务（Name Service）  保证了不同服务的命名都是唯一不冲突的
*   配置管理（Configuration Management）   利用观察者模式，当某一个服务配置发生改变时会通知其他依赖该服务的其他服务，解决配置改变导致其他系统崩溃问题
*   集群管理（Group Membership）  使用master-slaver 主从来管理集群，自动的生成master,解决master死掉之后无master问题。
*   共享锁（Locks）

所以zookeeper是必不可少的用来协调环境的框架，需要很好的理解。 <br>

入门博文推荐 <br>

[zookeeper分布式服务框架](http://blog.linuxeye.com/393.html) <br>
[zookeeper原理分析](http://cailin.iteye.com/blog/2014486)
