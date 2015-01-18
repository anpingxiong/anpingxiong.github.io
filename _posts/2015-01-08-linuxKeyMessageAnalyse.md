---
layout: post
category: ubuntu
title:  常用命令输出信息详解　
tagline: by anping
tags: [ubuntu key, 命令详解]
---
top
===
进程使用cpu，内存的动态显示过程.


    top - 11:36:33 up 3 days, 21:41,  3 users,  load average: 0.48, 0.44, 0.66
    Tasks: 244 total,   2 running, 239 sleeping,   0 stopped,   3 zombie
    %Cpu(s):  9.5 us,  2.3 sy,  0.0 ni, 88.0 id,  0.2 wa,  0.0 hi,  0.0 si,  0.0  st
    KiB Mem:  16340284 total, 10035756 used,  6304528 free,   368528 buffers
    KiB Swap: 16680956 total,        0 used, 16680956 free.  3932760 cached Mem
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    15267 anping    20   0 2097572 460460  62484 R  39.6  2.8  40:05.53 firefox  


*    top - 11:36:33 up 3 days, 21:41,  3 users,  load average: 0.48, 0.44, 0.66  等同于uptime　命令

　　　　>上面的显示表示当前时间是11:36:33，电脑登陆已经有３天，登陆时间是21:41,当前有三个用户登陆，
      本台机器的平均负载是一分钟0.48,五分钟0.66,十五分钟是0.66,负载越高，表示机器压力越大
　　　

*    KiB Mem:  16340284 total, 10035756 used,  6304528 free,   368528 buffers　等同于free命令


　　　　>上面的命令表示总共的内存大小是16G .已经使用接近10G,剩余大概6Ｇ,有大概3G是用于创建buffers缓冲区用于加快系统文件存储速度，有大概3Gcached缓存区用于加快系统的读取速度


ifconfig
========

    eth0      Link encap:以太网  硬件地址 f8:b1:56:e3:15:16  
    inet 地址:10.237.107.45  广播:10.237.107.255  掩码:255.255.255.0
    inet6 地址: fe80::fab1:56ff:fee3:1516/64 Scope:Link
    UP BROADCAST RUNNING MULTICAST  MTU:1500  跃点数:1
    接收数据包:20305435 错误:0 丢弃:0 过载:0 帧数:0
    发送数据包:18363859 错误:0 丢弃:0 过载:0 载波:0
    碰撞:0 发送队列长度:1000
    接收字节:6380770304 (6.3 GB)  发送字节:2168953717 (2.1 GB)
    中断:20 Memory:f7c00000-f7c20000


*   Link encap:以太网 :表示使用的是万维网
*   硬件地址 f8:b1:56:e3:15:16 :表示mac地址是 f8:b1:56:e3:15:16
*   inet 地址:10.237.107.45　：表示ip地址是　10.237.107.45
*   掩码:255.255.255.0 : 表示子网掩码
*   在学校经常需要手动的配置ip ,则在linux下使用命令配置的方式是:


    fconfig eth0 210.34.6.89 netmask 255.255.255.128 broadcast 210.34.6.127


netstat
=======
主要用于Linux察看自身的网络状况，如开启的端口、在为哪些用户服务，以及服务的状态等

netstat -a  同时还显示端口状态


telnet
======
远程登陆

telent 1p 端口　可以用来检测某一台机器的端口是否开放


nslookup
========

查询ip

    nslookup www.baidu.com
    ping www.baidu.com



grep
====

对于文本或者是终端输出的内容做一次查询过滤


博文推荐：[常用命令详解](http://blog.chinaunix.net/uid-16728139-id-3154272.html)
