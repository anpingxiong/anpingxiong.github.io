---
layout: post
category: JAVA
title: 动态代理回顾
tagline: by anping
tags: [JAVA,代理]
---



在学习spring 的时候，自己练习过JDK的动态代理，后来时隔到现在只是记得在jdk :java.lang.reflect 包中有代理proxy类，今天看spring 的aop 核心的时候，介绍到有两种常用的代理实现方式.

实现如下:

1.	jdk  动态代理，依赖接口实现。

2.	借助cglib 实现。



JAVA proxy API:

	Foo f = (Foo) Proxy.newProxyInstance(Foo.class.getClassLoader(),new Class[] { Foo.class },hanlder);


在加上描述FOO为代理接口，我只需要做一个handler(InvocationHandler)的实现在传入需要代理的对象即可。然后如上调用即可产生相应的代理类。
如下:


	@Override
	public Object invoke(Object proxy, Method method, Object[] args)
	throws Throwable {

		System.out.println(“代理开始”);
		// TODO Auto-generated method stub
		// method.invoke(proxy, args);
		Object object = method.invoke(target, args);
		System.out.println(“代理结束”);
		return object;

	}



incoke 方法hanlder必须实现的方法，主要是拦截需要代理的对象的方法，如何做些处理。但是我凌乱了，因为那个proxy 参数是碰不得的，一碰就死循环。必须使用你需要代理的对象，让它来调用方法才行。

好吧，我现在也没有搞清楚proxy参数 有什么用处～～～～但是proxy 却是可以调用方法的时候会自动的先调用这个invoke 方法，并把需要调用的方法和参数传输给invoke方法！



代码测试proxy
-------------


1.	定义了一个Target接口：

	public interface Target {
	public void show();
	}


2.	实现接口类，TargetImpl:

	public class TargetImpl implements Target{
		public void show(){
		   System.out.println(“show被调用了”);
		}
	}


3.	实现InvocationHandler类：
	public class MyInvocationHandler implements InvocationHandler {
		private Object target;
		public MyInvocationHandler(Object object){
		this.target=object;

		}
		public Object getProxy(Class<?> interfaceName){
		return Proxy.newProxyInstance(this.target.getClass().getClassLoader(),new       Class[]{interfaceName},this);
		}

		@Override
		public Object invoke(Object proxy, Method method, Object[] args)
		throws Throwable {

		System.out.println(“代理开始”);
		// TODO Auto-generated method stub
		// method.invoke(proxy, args);
		Object object = method.invoke(target, args);
		System.out.println(“代理结束”);
		return object;

		}

	}


4.	测试：

	Target proxy = (Target) new MyInvocationHandler(new TargetImpl()).getProxy(Target.class);

	System.out.println(proxy instanceof Target);
	proxy.show();


小结：代理的方式有很多：
-----------------------
1.	我们可以使用继承来实现代理。

2.	代理对象和需要代理对象实现同一个接口，代理对象来持有需要代理的对象来实现代理。

3.	jdk提供的代理，这个面向接口的是更加的灵活，因为可以针对不同的实现来做代理。

4.	cglib来做代理哦。（只是看过它来产生类，不是很了解,但是它十分的像继承来实现代理，只不过是通过父类字节码的方法来创建代理对象）。
