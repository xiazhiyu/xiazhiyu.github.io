---
layout: post_cn
title: "用python构建restful服务"
date: 2014-12-14 21:31:36
categories: 开发
tag: python
---

跟java比起来python效率是很快的。

首先，通过virtualenv建立虚拟环境。
  {% highlight sh %}
virtualenv ENV
  {% endhighlight %}

进入创建好的ENV文件夹，启动虚拟环境。
  {% highlight sh %}
cd ENV
source ./bin/activate
  {% endhighlight %}

安装flask-restful。
  {% highlight sh %}
pip install flask-restful
  {% endhighlight %}
  
