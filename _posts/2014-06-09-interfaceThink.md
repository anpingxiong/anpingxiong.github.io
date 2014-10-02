---
layout: post
category: JAVA
title: 内部类的妙用
tagline: by anping
tags: [内部类,接口]
---



在微博中看到一个网友提出的一个怪问题是在如下代码中添加遗憾代码要求输出:
Hello  W

olrd



	if(){  
	      
		System.out.print(“Hello”);  
			        
	}else{  
							      
		System.out.print(“World”);  
									        
	}



在ｉｆ中写你的代码来完成输出。

然后当时立马想到的是使用

	if(System.out.print(“Hello “)==null)

但是输出的是Hello Wolrd.结果是不对的！

网友有这样写的
-------------

	new　Callable<Boolean>(){  
	  
		public boolean call(){  
		    
			/×××some code**/  
			  
			 System.exit(0);  
			    
			return false;  
				  
	  }  
	    
	}.call()==false 




当时很惊讶，虽然内部类知道怎么用，但是实际上一般只是在做监听器的时候才用过。（自己很木讷，不知道灵活应用）

确实感触很多，因为if中需要的是返回的是布尔型的表达式（让我想起了加强for）。

所以对于四种内部类的灵活利用很重要。

并且接口在设计模式中大量的使用，如代理模式，模式模式等等。。。

所以java项目开发时面向接口编程会使项目更加的灵活。
