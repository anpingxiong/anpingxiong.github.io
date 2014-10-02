---
layout: post
category: 算法
title: 快排总结
tagline: by Snail
tags: [sort ,QuickSort ,快排 ,查找]
---

	#include <stdio.h>  
	/* 
	成功！ 
	*/  
	void main(){  
		int i=0;  
		   
		int group(int a[],int start,int end);  
	    	void  quickSort(int a[],int start,int end);  
	    	int  a[]=   {1,8,1,8,1,0};  
			quickSort(a,0,5);  
		    for(;i<=5;i++){  
		    	printf("%d",a[i]);  
			}  
								    
									  
	}


##介绍
这次写的算法是快速排序
快排，不单单是代码简单而却平均效率也是不错的,是一个蛮通用的排序.


##快排的主要特点是分组:
通过像是二叉树样的分组下去,知道分到只是剩下一个数,有点像归并也是就地排序.
因为是分组 那么核心就是分组 哦,因为是就地排序 所有是不变的一个数组 a[].然后是需要以哪个数组小标做分组的起始 start.


##犯下的严重错误：

    anping=anping^dandan;
	dandan=anping^dandan;
    anping=anping^dandan;
				    
这是一个相当快的交换值的算法,我屁颠屁颠的用在了这里 ,但是不曾想,当i和j指向的是同一个下标的时候,交换来的值却都是0。



##获取元素最终位置代码



	int   group(int a[],int start,int end){  
		int temp;  
		int i=start-1; //i的值是用来记录当前 大值的最后位置   
		int j=start; //来遍历数组  
		for(;j<end;j++){  
			if(a[j]<=a[end]){ //如果小于的话可以交换 与大值位置交换 以便在小值部分区域  
				i++;  
				temp=a[j];  
				a[j]=a[i];  
				a[i]=temp;  
		    }  
										     
		}  
											    
		temp=a[i+1] ;  
	    a[i+1]=a[end] ;  
		a[end]=temp;  
	    return  i+1;  
    }




	void quickSort(int a[],int start,int end){  
	    /*初始位置小于结束位置*/  
	          
    	if(start<end){  
			int  pos= group(a,start,end);  
		  	quickSort(a,start,pos-1); //注意这个  
			quickSort(a,pos+1,end);   //注意这个pos+1  
		}else{  
			return;  //这是终点  但是要不要无所谓的！  
		}  
													            
	} 
