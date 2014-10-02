---
layout: post
category: Apache
title: 使用apache构建tomcat集群问题及解决
tagline: by anping
tags: [apache]
---


在使用 apache 2.4.6和tomcat集成的时候，刚开始是编译了mod_jk模块来集成tomcat.

在按照文档（官方＋网上搜索）来配置的时候需要配置balanceMembership:

##部分配置过程

	#配置在workers.properties文件中
	#workers.properties
	#配置格式为worker.<worker name>.<directive>=<value>
	#
	# worker列表
			worker.list=lb_s,status
	# 第一个tomcat
	# ————————
	#port 为配置tomcat ajp监控端口，不是http的端口
			worker.s1.port=8080
	#tomcat的主机地址，如不为本机，请填写ip地址
			worker.s1.host=localhost
			worker.s1.type=ajp13
	#负载的权重值，越高表示负载越大
			worker.s1.lbfactor=1
	# 第二个tomcat
	# ————————
			worker.s2.port=8888
			worker.s2.host=localhost
			worker.s2.type=ajp13
			worker.s2.lbfactor=1
	# 第N个tomcat
	# ————————
	#worker.sN.port=10009
	#worker.sN.host=localhost
	#worker.sN.type=ajp13
	#worker.sN.lbfactor=1
	#用于负载均衡分发的控制器,名称为lb_s
			worker.lb_s.type=lb
	#失败时重试转发次数
			worker.lb_s.retries=3
	#加入负载均衡的tomcat worker,上面定义如要加载在这里
			worker.lb_s.balanced_workers=s1,s2
	#配置session会话是否为粘性
	#这样负载均衡器lb就会尽量保持一个session，也就是使用户在一次会话中跟同一个Tomcat进行交互
	#不建议配置为1(or true)
	#worker.lb_s.sticky_session=false
	#worker.lb_s.sticky_session_force=true
	#设置运行状态的控制器
			worker.status.type=status



由上面的配置可以知道均衡到两个tomcat上，分别占用了端口8080,8888.但是在启动tomcat和apache httpd的时候，发现根本就导航不到tomcat中，查看日志的时候只是报出client notfound.

悲剧的是在网上找不到解决方案。

那么我放弃使用mod_jk来做，而是使用反向代理的方式（详见文档），那么主要需要配置如下：

##配置在httpd-vhosts.conf文件中


	<VirtualHost *:80>
	ServerAdmin 974984076@qq.com
	ServerName 127.0.0.1:80
	ServerAlias localhost
	ProxyPass / balancer://cluster/ stickysession=JSESSIONID|jsessionid nofailover=Off
	ProxyPassReverse / balancer://cluster/
	ErrorLog “logs/lbtest-error.log”
	CustomLog “logs/lbtest-access.log” common
	<Proxy balancer://cluster>
	BalancerMember ajp://127.0.0.1:8080 loadfactor=1 route=tomcat1
	BalancerMember ajp://127.0.0.1:8888loadfactor=2 route=tomcat2
	</Proxy>

	</VirtualHost>


##问题出现

配置结束之后还是出现找不到的问题。

##解决过程

实在没有了办法，为了测试是apache 的问题还是tomcat配置的问题httpd-vhosts.conf做了如下修改配置：


	ProxyPass   / http://localhost:8080/


##看到希望

终于访问成功了。响应头包含如下数据：

    Server:
	    Apache/2.4.6 (Unix)
    Set-Cookie:
	    JSESSIONID=662E8CDA82F32E1B5D8F9613EF86845F.tomcat2; Path=/testCluster/; HttpOnly

##分析原因
那么修改成http之后就生效了，立即明白是错误在这里


	BalancerMember ajp://127.0.0.1:8888loadfactor=2 route=tomcat2				


通过查阅tomcat的servel.xml的配置发现ajp协议的端口是8019.而ajp协议是适合对tomcat和其他服务器集成使用的。而8888端口是遵守的协议是http

那么有如下两种修改方式：

	BalancerMember ajp://127.0.0.1:8019 loadfactor=2 route=tomcat2//更适合tomcat的集成

	BalancerMember http://127.0.0.1:8888 loadfactor=2 route=tomcat2//不建议tomcat的集成


同理在使用mod_jk　方式来负载均衡的时候，worker.sN.type=ajp13，也是使用了ajp协议，那么就需要把端口调整好来。

