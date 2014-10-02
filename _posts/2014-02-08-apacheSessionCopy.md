---
layout: post
category:apche
title: tomcat集群session无法复制的解决
tagline: by anping
tags: [apche,session]
---


在将自己的项目做负载均衡时，使用apache 2.4.6和tomcat 7.0. 

##配置过程

在做session复制时使用如下的配置。

1.	在tomcat中tomcat路径下%／config/servel.xml中配置cluster标签：


	<Service name="Catalina">

		<!--The connectors can use a shared executor, you can define one or more named thread pools-->
		<!--
		<Executor name="tomcatThreadPool" namePrefix="catalina-exec-"
			maxThreads="150" minSpareThreads="4"/>
		-->


		<!-- A "Connector" represents an endpoint by which requests are received
			 and responses are returned. Documentation at :
			 Java HTTP Connector: /docs/config/http.html (blocking & non-blocking)
			 Java AJP  Connector: /docs/config/ajp.html
			 APR (HTTP/AJP) Connector: /docs/apr.html
			 Define a non-SSL HTTP/1.1 Connector on port 8080
		-->
		<Connector port="8080" protocol="HTTP/1.1"
				   connectionTimeout="20000"
				   redirectPort="8443" URIEncoding="UTF-8"/>
		<!-- A "Connector" using the shared thread pool-->
		<!--
		<Connector executor="tomcatThreadPool"
				   port="8080" protocol="HTTP/1.1"
				   connectionTimeout="20000"
				   redirectPort="8443" />
		-->
		<!-- Define a SSL HTTP/1.1 Connector on port 8443
			 This connector uses the JSSE configuration, when using APR, the
			 connector should be using the OpenSSL style configuration
			 described in the APR documentation -->
		<!--
		<Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
				   maxThreads="150" scheme="https" secure="true"
				   clientAuth="false" sslProtocol="TLS" />
		-->

		<!-- Define an AJP 1.3 Connector on port 8009 -->
		<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />


		<!-- An Engine represents the entry point (within Catalina) that processes
			 every request.  The Engine implementation for Tomcat stand alone
			 analyzes the HTTP headers included with the request, and passes them
			 on to the appropriate Host (virtual host).
			 Documentation at /docs/config/engine.html -->

		<!-- You should set jvmRoute to support load-balancing via AJP ie :
		<Engine name="Catalina" defaultHost="localhost" jvmRoute="jvm1">
		-->
		<Engine name="Catalina" defaultHost="localhost"  jvmRoute="tomcat1">

		  <!--For clustering, please take a look at documentation at:
			  /docs/cluster-howto.html  (simple how to)
			  /docs/config/cluster.html (reference documentation) -->
		  <!--
		  <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>
		  -->

		  <!-- Use the LockOutRealm to prevent attempts to guess user passwords
			   via a brute-force attack -->
		  <Realm className="org.apache.catalina.realm.LockOutRealm">
			<!-- This Realm uses the UserDatabase configured in the global JNDI
				 resources under the key "UserDatabase".  Any edits
				 that are performed against this UserDatabase are immediately
				 available for use by the Realm.  -->
			<Realm className="org.apache.catalina.realm.UserDatabaseRealm"
				   resourceName="UserDatabase"/>
		  </Realm>

		  <Host name="localhost"  appBase="webapps"
				unpackWARs="true" autoDeploy="true">

			<!-- SingleSignOn valve, share authentication between web applications
				 Documentation at: /docs/config/valve.html -->
			<!--
			<Valve className="org.apache.catalina.authenticator.SingleSignOn" />
			-->

			<!-- Access log processes all example.
				 Documentation at: /docs/config/valve.html
				 Note: The pattern used is equivalent to using pattern="common" -->
			<Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
				   prefix="localhost_access_log." suffix=".txt"
				   pattern="%h %l %u %t "%r" %s %b" />

		 
	 
		  </Host>

		   
		
		  <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster" 

					 channelSendOptions="8">  

			  <Manager className="org.apache.catalina.ha.session.DeltaManager" 

					   expireSessionsOnShutdown="false" 

					   notifyListenersOnReplication="true"/>  

	 

			  <Channel className="org.apache.catalina.tribes.group.GroupChannel">  

				<Membership className="org.apache.catalina.tribes.membership.McastService" 

			   address="228.0.0.4"                port="45564"   

							frequency="500" 

							dropTime="3000"/>  

				<Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver" 

						  address="auto" 

						  port="4000"  

						  autoBind="100" 

						  selectorTimeout="5000" 

						  maxThreads="6"/>  

				

				<Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter">  

				  <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender" />  

				</Sender>  

				<Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/>  

				<Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatch15Interceptor"/>  

			 <Interceptor className="org.apache.catalina.tribes.group.interceptors.ThroughputInterceptor"/>  

			  </Channel>  

			  <Valve className="org.apache.catalina.ha.tcp.ReplicationValve" 

					 filter=""/>  

			  <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>  

	 

			  <Deployer className="org.apache.catalina.ha.deploy.FarmWarDeployer" 

						tempDir="/tmp/war-temp/" 

						deployDir="/tmp/war-deploy/" 

						watchDir="/tmp/war-listen/" 

						watchEnabled="false"/>  

			  <ClusterListener className="org.apache.catalina.ha.session.JvmRouteSessionIDBinderListener"/>  

			  <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener"/>  
		 </Cluster>
	 

	  </Engine>
	</Service>



2.	进入项目　中 
%tomcat路径下%／webapps/项目/web_inf/web.xml配置如下：

	<distributable/>  
但是在运行项目登录是因为在　％ apache路径％／config/extra/httpd-vhosts.conf中配置了：
	ProxyPass / balancer://cluster/ stickysession=JSESSIONID|jsessionid nofailover=Off


即使用session复制而不使用在第一次访问了某一个应用服务器时就一直访问那个服务器。而是每个服务器都拥有一样的session,可根据压力有apache均衡访问。


##问题出现
	遗憾的是一直没有登录成功，session没有复制成功。 

##解决：

在做一番资料查询之后，终于找到了方式，除了需要做以上所有的配置（一定要配置）之后，还需要在tomcat路径下%／config/context.xml配置如下：


	<?xml version='1.0' encoding='utf-8'?>
	<!--
	  Licensed to the Apache Software Foundation (ASF) under one or more
	  contributor license agreements.  See the NOTICE file distributed with
	  this work for additional information regarding copyright ownership.
	  The ASF licenses this file to You under the Apache License, Version 2.0
	  (the "License"); you may not use this file except in compliance with
	  the License.  You may obtain a copy of the License at

		  http://www.apache.org/licenses/LICENSE-2.0

	  Unless required by applicable law or agreed to in writing, software
	  distributed under the License is distributed on an "AS IS" BASIS,
	  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	  See the License for the specific language governing permissions and
	  limitations under the License.
	-->
	<!-- The contents of this file will be loaded for each web application -->
	<Context distributable="true">

		<!-- Default set of monitored resources -->
		<WatchedResource>WEB-INF/web.xml</WatchedResource>

		<!-- Uncomment this to disable session persistence across Tomcat restarts -->
		<!--
		<Manager pathname="" />
		-->

		<!-- Uncomment this to enable Comet connection tacking (provides events
			 on session expiration as well as webapp lifecycle) -->
		<!--
		<Valve className="org.apache.catalina.valves.CometConnectionManagerValve" />
		-->
	 
	</Context>



##提醒：

有时候在写代码的时候如写pojo  类时，总是懒得将她们实现序列化，（因为在使用像hirbernate或者mybatis时不写也不错，更加别说jdbc了），

当时确实不会出问题的，但是在一些项目需要使用strust-json插件是，将对象转化为流时会提示一定需要将对象实现序列化接口。

那么在本次负载均衡学习中更加是将为什么需要序列化pojo解释的很清楚，因为需要使用session复制，就需要将session中存放的对象拷贝到其他服务器。而此时就需要要将对象序列化之后才能在网络中传输。否则就连简单的登陆也实现不了！
