---
layout: post
category: ubuntu
title: 常用命令
tagline: by anping
tags: [ubuntu,命令]
---


在ubuntu下编程，怎么不知晓点命令,花点时间稍微的整理下。

ls
--

	ls  		  显示当前目录下的所有文件和文件夹
	ls -l         显示当前目录下的所有文件和文件夹信息，比如权限，所属用户,最后修改时间
	ls -i         显示当前目录的所有文件和文件夹以及 他们在系统中的索引号
	ls ./*        显示当前目录的所有文件和文件夹以及子文件夹下的所有文件和文件夹
	ls -r         反序显示
	ls -s         显示当前目录的所有文件和文件夹以及大小
	ls -t         通过文件的最后修改时间来排序显示
	ls -a         显示所有的隐藏文件

cat
---
	cat filename 		 显示filename文件中的内容
	cat -A filename 	 显示filename文件时将TAB内容以^I表示, 在行尾加上$符号
	cat -b filename		 显示fileaname文件时顺便显示行号
	cat -e filename      显示filename文件是在行尾加上$符号
	cat file1 >> file2   将文件1的内容追加在文件2的尾部
	cat file1 file2 >>file3 将文件1,文件2内容合并之后追加到file3中




head
----
	head -n 10 filename  显示filename前10行内容

tail
----
	tail -n 10 filename  显示filename尾部10行内容
	tail -f filename     跟踪显示文件末尾的内容
	tail -f filename | grep 'key' 跟踪显示文件中关键字为key的文件末尾内容

sed
---
	sed '2,5d' filename  显示filename文件的2～5行内容


rename
------
	rename 's/anping.txt/anping1.txt/' 将anping.txt更名为anping1.txt(该命令可以用正则方式修改)


cp
--
	cp file dir 		将file拷贝到dir目录中去 (不拷贝文件信息如权限和最后修改时间)
	cp -i file dir      将file拷贝到dir目录中(当发现有相同文件则提示是否覆盖)
	cp -f file dir      将file拷贝到dir目录中(如果有相同文件直接覆盖)
	cp -p file dir      将file拷贝到dir目录中(同时拷贝文件信息权限和最后修改时间等)
	cp -l file dir      不做拷贝只做链接,任何一方文件修改，所有文件都修改

mv
--
	mv anping.txt  anping1.txt  将anping.txt重命名为anping1.txt
	mv file dir 				将file移动到dir目录中
	mv -i file dir 				将file移动到dir目录中(如何有相同文件则提示是否覆盖 )
	mv -f file dir   			将file移动到dir目录中(如果有重复直接覆盖)

rm
--
	rm file   	删除file文件
	rm -i file  删除file文件前提示是否删除
	rm -f file  直接删除file文件无任何提示
	rm -r dir   删除文件夹及文件夹下的所有的文件

rmdir
-----
	rmdir dir   删除dir目录，但是前提是dir是空目录


touch
-----
	touch file		 用来修改文件时间，如果没有文件则创建file文件
	touch -c file  	 不创建file

echo
----
	echo "aaaa" >> anping.txt  将aaaa字符串追加到anping.txt文件中
	echo "aaaa"	 > anping.txt 	清空anping.txt并将aaaa写到文件中
