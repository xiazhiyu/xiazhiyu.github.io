---
title: タグ
layout: default_ja
---

<div id='tag_cloud'>
</div>

<ul class="listing">
{% for tag in site.tags %}
    <ul class="category">
    <li class="listing-seperator" id="{{ tag[0] }}">{{ tag[0] }}</li>
    {% for post in tag[1] %}
      {% if post.categories contains 'ja' %}
        <li class="listing-item">
        <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
        <a href="{{ site.baseurl }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
        </li>
      {% endif %}
    {% endfor %}
    </ul>
{% endfor %}
</ul>
