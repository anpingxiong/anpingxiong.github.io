---
layout: post
category: xml
title:  xml 
tagline: by anping
tags: [XML]
---


xml (extensible markup language) 
--------------------------------
可扩展标签语言
*	xml 是HTML的扩展，不是HTML的替代品。
*	xml 常用来数据交互，即传输数据.
*	html主要是用来显示数据。

普通自定义的简单XML中必须定义为树结构并含有一个根标签对.比如如下:
	

	<root>
		<.../>
		<.../>
	</root>



错误的xml定义(没有根标签):
	

	<anping></anping>
	<anping1></anping1>



XML 如果随便定义而没有规范的文档定义话，不利于在网络中用来通讯。因为不同的应用程序可能对未文档定义的xml解析方式不同，或者无法解析。
所有我们需要对xml做文档定义。

文档定义方式
------------

1.	DTD (Document Type Definition)
2.	Schema( Xml Schema)


DTD
---
DTD在对xml做定义时需要将定义信息保装在<!DOCTYPE>中,如:

	
	<!DOCTYPE anping[
	 <!ELEMENT anping(name,sex,age)>
	 <!ELEMENT  name(#PCDATA)>
	 <!ELEMENT  sex(#PCDATA)>
	 <!ELEMENT  age(#PCDATA)>
	]>
	<anping>
		<name>熊安平</name>
		<sex>男</sex>
		<age>22</age>
	</anping>


*	<!ELEMENT anping(name,sex,age)>指定了anping 这个根节点有name,sex,age三个属性
*	<!ELEMENT name(#PCDATA)> 指定了name 的类型为PCDATA  (PCDAT是表示需要被解析的字符数据，所以在name内容中不能包含特殊字符如">"等，而还有一种类型为CDATA,是表示无需将里面的数据做解析的)
*	其他类似name



DTD的优点
*	为文档的编制带来了便利，加强了xml文档的一致性。
DTD的缺点
*	DTD有自己的语法，和xml语法不同
* 	DTD只是提供了PCDATA和CDATA两种类型	



Schema
------
xml  Schema是以xml为基础的用来可替代DTD的类型定义架构
简单的代码如下:


	<?xml version="1.0"?>
	<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
	targetNamespace="http://www.w3school.com.cn"
	xmlns="http://www.w3school.com.cn"
	elementFormDefault="qualified">

	<xs:element name="note">
		<xs:complexType>
		  <xs:sequence>
		<xs:element name="to" type="xs:string" default="aa"/>
		<xs:element name="from" type="xs:string"/>
		<xs:element name="heading" type="xs:string"/>
		<xs:element name="body" type="xs:string"/>
		  </xs:sequence>
		</xs:complexType>
	</xs:element>

	</xs:schema>


在上面的代码中定义了命名空间xs
xs:element指名了元素的数据类型和默认值


优点:
-----
1.	支持数据类型，可以方便对数据类型定义，对数据转换和对数据验证
2.	使用xml语法，无需再去写其他语法
3.	可扩展




JAVA对xml解析的4种方式
----------------------

1.	DOM
DOM 是通过xml的树结构的方式去解析xml的。通过这种返式解析会在操作上带来很大的便利，但是缺点是需要加载整个XML,这对于大型的xml文档来讲是灾难的。	

2.	JAX

JAX 是一个java api for xml ，它是通过事件机制来解析xml的，当解析到某一个节点时就会触发事件，然后让程序员去处理，这样解决了需要全部加载至内存中的问题.
3.	JDOM

JDOM是的性能则介于DOM 和JAX ,使用了DOM的解析方式，并支持JAX的规范

4.	DOM4J

DOM4J 是一个理想的 xml的解析器，被大量的使用，他提供了上面三个标准的实现，并且支持xpath,用到了大量的JAVA集合类，对于JAVA程序员来讲，更加适用。




