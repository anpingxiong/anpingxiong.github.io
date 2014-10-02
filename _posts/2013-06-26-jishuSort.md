---
layout: post
category: 算法
title: 计数排序 
tagline: by anping
tags: [sort ,计数排序 ,排序]
---



	#include<stdio.h>  
    /*计数排序   一个稳定不需要比较 效率为线性的算法 
     
       基本原理：： 
            
              通过借助临时数组的下标来记录 需要排序数组中值排序后 
              的正确位置 
           
    */  
      
      
    /* 
     
       输入: a数组  k为数组中的最大值  length为a 数组的长度 
    */  
      
    void countSort(int a[],int k,int length){  
         int c[20];//用途是记录a数组中所有值所应该在的地方  
         int b[20];  
        //1----先是置0  
        for(int i= 0 ;i<20; i++){  
            c[i]=0 ;  
          
        }  
      //2---然后将a中的值数量以c中的下标对应记录  既c的下标代表a中的值,c中的值代表a中值为c下标的个数  
        for(int j=0 ; j<length;j++){  
           c[a[j]] =c[a[j]]+1;  
        }  
       //3--这是记录 a中值 在a数组中排序后所应该在的位置  
        for(int h= 0 ; h<k+1 ;h++){  
            c[h+1] = c[h+1]+c[h];  
            
        }  
      
         //4--排序到b数组中去  
        for(int d=length-1 ; d>=0;d--){  
                
            b[c[a[d]]-1]=a[d];  
              c[a[d]]=c[a[d]]-1;  
        }  
      
         //5--完毕  
        for(int q=0;q<length;q++){  
            a[q]=b[q] ;   
        }  
          
    }  
    void main(){  
        int  a[11]={1,2,6,1,9,1,9,2,4,2,5};  
        countSort(a,9,11);  
        for(int i= 0 ; i<11;i++){  
          printf("%d",a[i]);  
        }  
    }  
