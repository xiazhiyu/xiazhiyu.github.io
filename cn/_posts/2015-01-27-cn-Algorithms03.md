---
layout: post_cn
title: "算法练习03：简介(3)"
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
