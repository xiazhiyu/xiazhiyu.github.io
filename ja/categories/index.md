---
title: カテゴリ
layout: default_ja
---

<div id='tag_cloud'>

</div>

<ul class="listing">
{% for cat in site.categories %}
  {% if cat[0] != 'cn' and cat[0] != 'ja' %}
      <ul class="category">
      <li class="listing-seperator" id="{{ cat[0] }}">{{ cat[0] }}</li>
      {% for post in cat[1] %}
        {% if post.categories contains 'ja' %}
          <li class="listing-item">
          <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
          <a href="{{ site.baseurl }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
          </li>
        {% endif %}
      {% endfor %}
      </ul>
  {% endif %}
{% endfor %}
</ul>


<script src="/media/js/jquery.tagcloud.js" type="text/javascript" charset="utf-8"></script> 
<script language="javascript">
$.fn.tagcloud.defaults = {
    size: {start: 1, end: 1, unit: 'em'},
      color: {start: '#f8e0e6', end: '#ff3333'}
};

$(function () {
    $('#tag_cloud a').tagcloud();
});
</script>
