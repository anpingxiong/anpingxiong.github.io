---
layout: post
category: 算法
title: 枚举回顾
tagline: by anping
tags: [JAVA,枚举]
---


在jdk5.0 的时候enum 成为java 的新特性。

那在以前的版本中如果替代enum类呢？？

使用静态不变变量，如

	public  static  final String  exmple ="aaa";

枚举类是可以完全替代上述方式的，并且还支持switch语句，像string ,还是等到7.0 来支持switch语句。 但是枚举是不建议常用的，只有在大批的成员变量都市static final  的时候才建议使用。

怎么使用呢？？



	package DefineTag;

	public enum enumTest {
		January,
		February,
		March,
		April,
		May,
		June,
		July,
		August,
		march,
		October,
		November
	}


-----------------------------------


-----------------------------------

	package DefineTag;

	public class Main {
	public enumTest enum2;
	public Main(enumTest enum2){
	this.enum2 = enum2;
	}

	public static void main(String[] args) {
	enumTest test = enumTest.February;
	Main main = new Main(test);
	System.out.println(main.enum2);
	}
	}
