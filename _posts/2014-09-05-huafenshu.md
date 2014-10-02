---
layout: post
category: 算法
title: 划分数
tagline: by anping
tags: [划分数]
---



一个消耗空间和时间效率的，但是容易想到的划分数计算方式.

通过构建成递归树来完成，但是节点的值需要大于父节点的值。



代码
----

    public class HuaFen{  
       
          public int num; //需要划分的个数      
          //n表示需要划分的值， m表示迭代到的层数，count表示当前的总和  
          public  void  huaFen(int n,int m,int count){  
                
             for( ; m<=n;m++){  
                  //如果大于了我们需要的值则往上走吧，不需要再往下走了  
                  System.out.println("count"+count);  
                 if(count+m>n ){  
                 }  
      
                 if(count+m==n ){  
                     num++;  
                 }  
      
                 if(count+m<n){  
                     huaFen(n,m,count+m);  
                 }  
             }  
               
      
      
         }  
      
         
         public static void main(String args[]){  
      
             HuaFen huan = new HuaFen();  
             huan.huaFen(7,1,0);  
             System.out.println(huan.num);  
         }  
	}  




