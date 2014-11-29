---
layout: post
category: JAVA
title: 枚举回顾
tagline: by anping
tags: [枚举]
---


在jdk5.0 的时候enum 成为java 的新特性。

那在以前的版本中如果替代enum类呢？？

使用静态不变变量，如

	public  static  final String  exmple ="aaa";

枚举类是可以完全替代上述方式的，并且还支持switch语句，像string ,还是等到7.0 来支持switch语句。 但是枚举是不建议常用的，只有在大批的成员变量都市static final  的时候才建议使用。

怎么使用呢？？

简单示例
-------



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



有时候通过枚举来定义常量还是不够的，比如Color 枚举可以定义red ,green 等，但是我们希望能够讲颜色搭配值放在里面，比如 Red(100,100,100)。

那么很简单，只是需要定义一个私有的构造方法。当然也可以提供getValue方法来获取里面的值。


enum赋值示例
----------




    package com.xiaomi.shank.user.util;

		import com.xiaomi.miliao.xcache.XCache;

		public enum QuotaCodeType {
			DAY(XCache.EXPIRE_DAY), HOUR(XCache.EXPIRE_HOUR), FIVEMINU(XCache.EXPIRE_MINUTE * 5);

			private int value;

			private QuotaCodeType(int value) {
				this.value = value;
			}

			public int getValue() {
				return this.value;
			}
		}
