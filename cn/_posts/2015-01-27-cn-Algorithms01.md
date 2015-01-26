---
layout: post_cn
title: "算法练习01：简介 最小可用ID (1)"
date: 2015-01-27 01:31:40
categories: 开发
tag: 算法
---

说来惭愧，因为自己不是科班出身，从来没系统学习过算法，工作中也似乎很少用到也没怎么研究。最近离职，准备利用空余时间准备系统学习一下。
主要是参考[初等算法](https://sites.google.com/site/algoxy/home/zh-cn)，一个国人写的教材来学习。(觉得自己实在太渣，不敢挑战难度太高的。)

本文为前言部分的练习代码。

第一题：假设我们使用非负整数作为某个系统的的ID，所有用户都由一个ID唯一确定。任何时间，这个系统中有些ID处在使用中的状态，有些ID则可以用于分配给新用户。现在的问题是，怎样才能找到最小的可用ID呢？例如下面的列表记录了当前正在被使用的ID：
[18, 4, 8, 9, 16, 1, 14, 7, 19, 3, 0, 5, 2, 11, 6]

1 暴力解法：
  {% highlight python %}
#!/usr/bin/python
# -*- coding: utf-8 -*- 
import time

lst = [18, 4, 8, 9, 16, 1, 14, 7, 19, 3, 0, 5, 2, 11, 6]

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
def brute_force(lst):
  i = 0
  while True:
    if i not in lst:
      return i
    i = i + 1

brute_force(lst)
  {% endhighlight %} 

为了测试执行时间，使用了python装饰器来计算执行时间。

2 改进1(python)
  {% highlight python %}
#方法2：第一次改进
@timeCul
def min_free(lst):
  lstLen = len(lst)
  fList = []
  for i in range(0, lstLen+1):
    fList.append(False)

  for i in lst:
    if i < lstLen:
      fList[i] = True

  for i in range(0, lstLen+1):
    if fList[i] == False:
      return i

min_free(lst)
  {% endhighlight %} 

[下篇](/cn/%E5%BC%80%E5%8F%91/2015/01/27/cn-Algorithms02.html)将使用c语言和二进制来进行改进。
