---
title: 日本語
layout: default_ja
---

<ul class="listing">
{% for post in site.posts %}
  {% if post.categories contains 'ja' %}
    {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
    {% if year != y %}
      {% assign year = y %}
      <li class="listing-seperator">{{ y }}</li>
    {% endif %}
    <li class="listing-item">
      <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
      <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
    </li>
  {% endif %}
{% endfor %}
</ul>

