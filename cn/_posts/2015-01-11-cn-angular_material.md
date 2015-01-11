---
layout: post_cn
title: "关于Angular Material的Text Field"
date: 2015-01-11 21:31:36
categories: 开发
tag: Angular
---

[Angular material](https://material.angularjs.org)是一个按照Google的Material Design建的组件框架。
目前稳定版本是0.6.1，网站文档也是0.6.1。而最新版本则是v0.7.0-rc2。
对于Text Field，0.7与0.6相比比较大。取消了md-text-float。
以前是这么写的：
  {% highlight javascript %}
<md-text-float label="LastName" ng-model="user.lastName"> </md-text-float>
  {% endhighlight %} 
angular会将md-text-float转换成一个label和input。
而现在则变成这样：
  {% highlight javascript %}
<md-input-container>
  <label>LastName</label>
  <input type="text" ng-model="user.lastName">
</md-input-container>
  {% endhighlight %} 
新版本除了input之外还支持textarea，在md-input-container内输入textarea也是可以的。
但是如果想用select呢？如果需要的话只能自己修改源码了。找到源码中如下部分（大约3800多行）：

  {% highlight javascript %}
angular.module('material.components.input', [
  'material.core'
])
  .directive('mdInputContainer', mdInputContainerDirective)
  .directive('label', labelDirective)
  .directive('input', inputTextareaDirective)
  .directive('textarea', inputTextareaDirective);
  {% endhighlight %} 

将它做如下修改
  {% highlight javascript %}
angular.module('material.components.input', [
  'material.core'
])
  .directive('mdInputContainer', mdInputContainerDirective)
  .directive('label', labelDirective)
  .directive('input', inputTextareaDirective)
  .directive('textarea', inputTextareaDirective)
  .directive('select', inputTextareaDirective);
  {% endhighlight %} 

接着需要自己对select做CSS样式修改。