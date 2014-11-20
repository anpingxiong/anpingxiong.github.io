---
layout: post
category: JAVA
title:  mock 4 java 学习
tagline: by anping
tags: [easymock,jmockit,mock,JAVA]
---


Mock 即模仿。在初次见识到mock时，并不知道有多大的作用，只是隐约知道这是用来生成一个自己想到的
对象。 我的想法确实没有错误，在项目中，常常需要去对某一个功能或者是对某一个对象做测试，但是同时
该功能常常需要依赖其他服务才能完成，但是其他服务的实现细节不是我们需要了解的，并且不一定能在测
试过程中被使用的，那么我们可以趣模仿他，去mock他，让我们的方法调用我们的对象儿不是它。


mock 4 java
===========


实际上mock 4java 的库很多，但我常用的有如下两个:

easymock[访问easyMock好文!](http://macrochen.iteye.com/blog/298032)


jmockit[访问jmockit!](http://baike.baidu.com/view/3018557.htm?fr=aladdin)


我对easymock 和jmockit 的理解
==========================

*    easymock很强大，通过它可以为我们mock的对象可以定义方法的返回值或者自己mock任意参数。而它的实现原理是使用了java 动态代理。
*    jmockit 可以做easymock的任何东西，但是它可以强大到直接修改虚拟机中的字节码，直接讲你想要mock的对象的字节码自己在虚拟机定义一遍，即自己实现需要mock对象的方法逻辑。
