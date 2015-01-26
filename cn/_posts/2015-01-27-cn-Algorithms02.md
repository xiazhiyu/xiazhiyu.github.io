---
layout: post_cn
title: "算法练习02：简介(2)"
date: 2015-01-27 01:35:33
categories: 开发
tag: 算法
---

接着[上篇](/cn/%E5%BC%80%E5%8F%91/2015/01/27/cn-Algorithms01.html)的练习。

3 改进2(c)

惭愧，没学过c，只好边看边学，顺便复习一下二进制。
  {% highlight c %}
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define N 1000000
#define WORD_LENGTH sizeof(int) * 8

void setbit(unsigned int* bits, unsigned int i)
{
  bits[i/WORD_LENGTH] |= 1 <<(i % WORD_LENGTH);
}

int testbit(unsigned int* bits, unsigned int i)
{
  return bits[i/WORD_LENGTH] & 1<<(i % WORD_LENGTH);
}

unsigned int bits[N/WORD_LENGTH + 1];


int xs[15] = {18, 4, 8, 9, 16, 1, 14, 7, 19, 3, 0, 5, 2, 11, 6};

int min_free(int* xs, int n)
{
  int i, j, len = N/WORD_LENGTH+1;//确定初始化bit数组长度
// 100000/16 +1 = 6251
  for(i = 0; i < len ; ++i)
  {
    bits[i]=0;//初始化bits数组,全部填充0
  }

  for(i=0; i< n; ++i)
  {
    printf("%d\n",i);
    if(xs[i]<n)
    {
      setbit(bits, xs[i]);
    }
  }

  for(i=0; ; ++i)
  {
    if(~bits[i]!=0){
      for(j=0;;++j){
        if(!testbit(bits,i*WORD_LENGTH+j))
        {
          return i*WORD_LENGTH+j;
        }
      }
    }
    
  }
  return -1;
}


int main(){
  int rs;
  clock_t clockT1, clockT2;
  clockT1 = clock();
  rs = min_free(xs, 15);
  printf("结果是: %d\n", rs);
  clockT2 = clock();
  printf("执行时间：%f秒\n",
                (double)(clockT2 - clockT1)/CLOCKS_PER_SEC);
  exit(0);
}
  {% endhighlight %} 

4 改进3(c)

实际是使用了二分法。

  {% highlight c %}
void swap(int *x, int *y)
{
  int t;
  t=*x;
  *x=*y;
  *y=t;
}

int min_free2(int* xs, int n){
  printf("begin xs size:%lu\n", sizeof(xs));
  int l = 0;
  int u = n-1;
  while(n){
   
    printf("%d\n", n);
    int m = (l+u)/2;
    int right, left = 0;
    for (right = 0; right < n ; ++ right){
      // printf("%d\n", m);
      // printf("%d\n", xs[left]);
      // printf("%d\n", xs[right]);
      // printf("%d\n", left);
      if(xs[right] <= m){
        swap(&xs[left], &xs[right]);
        ++left;       
      }
    }
    printf("%d\n", left);
    printf("xs size:%lu\n", sizeof(xs));
    printf("end for\n");
    printf("%d\n", left);
    if(left == m -l +1){
      printf("goto right\n");
      printf("%lu\n", sizeof(xs));
      xs = xs + left;
      printf("%lu\n", sizeof(xs));
      n = n - left;
      l = m + 1;
    }else{
      n = left;
      u = m;
    }
  }
  return l;
}
  {% endhighlight %} 
c的指针果然是难以理解。

做一下记录：
<p>
<code>
  xs = xs + left;
</code>
</p>
数组的指针指向的是数组中第一个元素（下标0）的地址，当指针xs加left个单位后，下标将增加left个单位，及下标0+left位置。当while第二次循环时，xs下标位0+left，接着执行完毕后会再增加left个单位，直到结束。
