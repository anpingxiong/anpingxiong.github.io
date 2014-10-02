---
layout: post
category: JAVA
title: 像文件中追加对象
tagline: by anping
tags: [ObjectOutPutStream]
---


在使用java来存储对象是很简单的，但是需要追加对象时为了方便在能使用ObjectInputStream读取所有对象时（包括追加的对象），就需要要做些特别的处理。



###首先能追加数据，那么在构造FileOutPutStream时注意,将append设置为true。

	FileOutputStream(File file, boolean append)

	创建一个向指定 File 对象表示的文件中写入数据的文件输出流。如果第二个参数为 true，则将字节写入文件末尾处，而不是写入文件开始处。创建一个新 FileDescriptor 对象来表示此文件连接。



###其次为了能保证追加的对象被读取到，那么需要自己去新建一个对象输出流，并继承ObjectInputStream,并重写writerStreamHeader方法。


	class ObjectOutputStreamForAddObject extendsObjectOutputStream {

		private static File f;

	 

	writeStreamHeader()方法是在ObjectOutputStream的构造方法里调用的由于覆盖后的writeStreamHeader()方法用到了f。如果直接用构造方法创建一个MyObjectOutputStream对象，那么writeStreamHeader()中的f是空指针因为f还没有初始化。所以这里采用单态模式(将构造方法定义为私有的，然后通过方法获取对象，可以保证某个类只能存在一个对象示例)

	 

		public static  ObjectOutputStreamForAddObject newInstance(File file,OutputStream out)

				 throws IOException {

			f = file;

			return new ObjectOutputStreamForAddObject(out);

	}

	 

		private ObjectOutputStreamForAddObject(OutputStream out) throws IOException{

			super(out);

		}

	 

		@Override

		protected void writeStreamHeader() throws IOException {

			if (!f.exists() || (f.exists() &&f.length() == 0)) {

				 super.writeStreamHeader();

			} else {

				 super.reset();

			}

		}

	}



###最后　在将对象写入文件的时候是调用ObjectOutputStreamForAddObject类，记得调用flush(),close()方法哦。
