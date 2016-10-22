---
layout: post
category: android
title: fragment 的onActivityResult 不被调用
tagline: by anping
tags: [fragment onActivityResult]
---

fragment 的onActivityResult 不被调用
---------------------

	
简介
====

使用Fragment 过程中总是会遇到各种各样的问题，比如不好判断当前可见fragment, fragment 的onActivityResult ,不能够处理fragment嵌套问题，fragment 的requesetCode 长度受到限制等等，


fragment 的onActivityResult ,不能够处理fragment嵌套问题 是因为activty的onActivityResult ,只是查询了一层fragment就没有查询了。


这个原因可以参考[Fragment中onActivityResult不被调用的解决方案](http://blog.csdn.net/shuaihj/article/details/46663109).


解决思路
========
	

*    1.重载项目中fragment基类中startActivityForResult，自动传入可以标示fragment的值token ,并且重载任何有setResult 的地方，通过EventBus发出结果，最后fragment 基类接受这个事件，通过token 判断是否需要处理，需要处理则调用自己的onActivityResult,
	这些逻辑需要写在fragment和activity基类中，并且借助EventBus 实现，很方便.

*    2.frament 中定义一个回调接口， 回调接口中用来返回setResult的结果，这个也需要在fragment中协议好，在启动frament 事需要传入接口的实现。

*    3.重载activity基类中的onActivityResult ,将只是查询一层fragment ,修改为递归查询，但是需要处理好，request是否被发起者处理了，而不是被其它也能处理这个requestCode 的fragment处理了。