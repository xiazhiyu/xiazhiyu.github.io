---
layout: post_cn
title: "算法练习03：简介 寻找丑数(1)"
date: 2015-01-27 05:00:33
categories: 开发
tag: 算法
---

接着[上篇](/cn/%E5%BC%80%E5%8F%91/2015/01/27/cn-Algorithms02.html)。

第二题：寻找第1500个“丑数”。所谓丑数，就是只含有2、3或5这三个因子的自然数。

1 暴力解法(python)

  {% highlight python %}
#!/usr/bin/python
# -*- coding: utf-8 -*- 

import time

#装饰器：打印执行时间
def timeCul(fn):
  def wrapper(lst):
    startTime = time.time()
    print 
    print "结果是: %s" % fn(lst) 
    print "%s 函数的执行时间：%s秒" %(fn.__name__, (time.time() - startTime))
  return wrapper

#方法1：暴力执行
@timeCul
def GetNumber(n):
  x = 1
  i = 0
  while True:
    if valid(x) == True :
      i = i + 1
      if i == n:
        return n
    x = x + 1


def valid(x):
  while x % 2 == 0:
    x = x / 2
  while x % 3 == 0:
    x = x / 3
  while x % 5 == 0:
    x = x / 5
  if x == 1:
    return True
  else:
    return False

GetNumber(1500)
  {% endhighlight %} 

python执行花了十几分钟！

2 改进1
  {% highlight python %}
#方法2：第一次改进
@timeCul
def GetNumberImprove(n):
  Q = [1]
  l = 0
  while n>0:
    x = Q[l]
    Q = UniqueEnqueue(Q, 2*x)
    Q = UniqueEnqueue(Q, 3*x)
    Q = UniqueEnqueue(Q, 5*x)
    n = n-1
    l = l + 1
  return x


def UniqueEnqueue(Q, x):
  i = 0
  while i < len(Q) and Q[i] < x:
    i = i + 1
  if i < len(Q) and x == Q[i]:
    return Q
  Q.insert(i, x)
  return Q
  {% endhighlight %} 
改进后仅花0.6秒！

3 改进3（c++）
  {% highlight c++ %}
#include <iostream>
#include <queue> 
#include <time.h>
using namespace std;

typedef unsigned long Integer;

Integer getNumber(int n){
  if(n == 1)
    return 1;
  queue<Integer> list2, list3, list5;
  list2.push(2);
  list3.push(3);
  list5.push(5);
  Integer x;
  while(n-- >1){
    x = min(min(list2.front(), list3.front()), list5.front());
    if(x == list2.front()){
      list2.pop();
      list2.push(x*2);
      list3.push(x*3);
      list5.push(x*5);
    }else if (x == list3.front()){
      list3.pop();
      list3.push(x*3);
      list5.push(x*5);

    }else if (x == list5.front()){
      list5.pop();
      list5.push(x*5);
    }
  }
  return x;
}

int main(){
  clock_t clockT1, clockT2;
  clockT1 = clock();
  printf("%lu\n",getNumber(1500));
  clockT2 = clock();
  printf("执行时间：%f秒\n",
                (double)(clockT2 - clockT1)/CLOCKS_PER_SEC);
  return 0;
}
  {% endhighlight %} 
执行时间：0.000211秒！

