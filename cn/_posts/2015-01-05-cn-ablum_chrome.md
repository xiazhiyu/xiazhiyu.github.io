---
layout: post_cn
title: "做了相册(chrome插件)"
date: 2015-01-05 21:31:36
categories: 开发
tag: chrome插件
---

之前曾做过一个相册。开发一个chrome的插件，将自己喜欢的图片收集起来，类似pinterest的东西。
不过当时是用php开发的。后来空间没有续费之后就没用了。

如今想想那东西还是很需要的。就用nodejs重新做了一个。
将程序放在[https://c9.io/](https://c9.io/)上。

依然用的瀑布流，不过这次用了一个国产框架。[http://wlog.cn/waterfall/index-zh.html](http://wlog.cn/waterfall/index-zh.html)

因为自己的相册是公开的，地址：[https://fyg-xiazhiyu.c9.io/](https://fyg-xiazhiyu.c9.io/)
想在页面中自己能够删除图片（又不许别人删除），又懒得开发注册验证之类，怎么办呢？
想了一个办法：
在chrome插件中，对相册网页body加入一个class。而在网页javascript中检查body是否有包含这个class，当存在时则显示删除按钮。
比较偷懒。呵呵。