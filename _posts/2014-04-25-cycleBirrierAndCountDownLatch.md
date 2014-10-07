---
layout: post
category: java
title:	CyclicBarrierAndCountDownLatch 
tagline: by anping
tags: [Thread,java]
---


多线程
------



countDownLatch
--------------
CountDownLatch 是JAVA中线程同步类.
会在一组线程完成之前，让一个或者多个线程处于等待状态。
常用场景:多个员工等待老板发布任务，同时开始做任务，完成之后，由老板汇总。
 
多个士兵等待上级发布命令，同时开始做任务，完成任务之后，有上级查看任务完成情况.
countDownLatch有两个重要的方法，分别是countDown,是指将countDownLatch初始化的值减一.
wawit方法是是线程处于堵塞状态，直到countDown为0.




比如:

	
	import java.util.concurrent.CountDownLatch;
	import java.util.concurrent.ExecutorService;
	import java.util.concurrent.Executors;
	public class CountDownLatchLearn {

		//假设main进程就是上级
		public static void main(String args[]) throws InterruptedException{
			
			CountDownLatch latch   = new CountDownLatch(3);//用来表示3个士兵的协作
			
			CountDownLatch latch2   = new CountDownLatch(1);//用来表示上级的发布命令
			ExecutorService  executer2= Executors.newFixedThreadPool(3);//java提供的线程池
			
			executer2.submit(new ShiBing(latch,latch2,"第一个士兵"));
			executer2.submit(new ShiBing(latch,latch2,"第二个士兵"));
			executer2.submit(new ShiBing(latch,latch2,"第三个士兵"));
		 
			Thread.sleep(200);
			System.out.println("上进程发布命令");
			latch2.countDown(); //将latch2 从1变成0,从而不再阻塞，士兵可以继续执行
			
			latch.await();//等待士兵执行完毕 即有3变成0
			System.out.println("收工");
			
			executer2.shutdown();
		}
	}

	class ShiBing implements Runnable{
		private CountDownLatch  latch  ;	
		private CountDownLatch  latch2  ;	
		
		private String name;
		public ShiBing(CountDownLatch latch,CountDownLatch latch2,String name){
			this.latch=latch;
			this.latch2=latch2;
			this.name=name;
		}
		
		public void run(){
			
			System.out.println(name+"等待任务");
			try {
				latch2.await();  //等待上级发布命令
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			System.out.println(name+"结束任务");
			latch.countDown();
		}
	}	








cyclicBirrier
-------------

cyclicBirrier也是线程同步类。是让所有的线程循环等待，直到达到某一个公共的屏障点。

常用的方法是await 。等待一直线程，直到所有的线程都准备好.
场景分析

比如同学一同来北京玩，则会一同先去长城，然后如果有人先下长城，则一起等待那些还没有下来的。在一同已故宫。



示例代码:

	import java.util.concurrent.BrokenBarrierException;
	import java.util.concurrent.CyclicBarrier;
	import java.util.concurrent.ExecutorService;
	import java.util.concurrent.Executors;


	public class TaskExceutor {

		public static void  main(String args[]) throws InterruptedException, BrokenBarrierException{
			CyclicBarrier  cy  = new CyclicBarrier(3);
			ExecutorService  exceutor =  Executors.newFixedThreadPool(3);
			exceutor.submit(new Student(cy, "同学1"));
			exceutor.submit(new Student(cy, "同学2"));
			exceutor.submit(new Student(cy, "同学3"));
		 
			
			exceutor.shutdown();
			 

		}
		
	}


	class  Student  implements Runnable{
		
		private CyclicBarrier  cyclicBarrier;
		private String StudentName;
		public	Student(CyclicBarrier cyclicBarrier,String name){
			this.StudentName  = name;
			this.cyclicBarrier  = cyclicBarrier;
		} 
		
		public void run(){	
			
			try {
				System.out.println(StudentName+"一起爬长城");
				cyclicBarrier.await();
				Thread.sleep(2000);
				System.out.println(StudentName+"等大家下长城");
				
				cyclicBarrier.await();
				Thread.sleep(2000);
				System.out.println(StudentName+"一起去故宫");
				 
				 
			} catch (InterruptedException e) {
				e.printStackTrace();
			}catch(BrokenBarrierException e1){
				e1.printStackTrace();
			}
			
		}
		
	}




cycliBarrier和CountDownLatch的区别
----------------------------------

与 CyclicBarrier 不同的是，CountdownLatch 不能重新使用.
