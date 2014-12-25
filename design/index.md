---
layout: page
title: "Design"
date: 
modified:
excerpt:
tags: []
image:
  feature:
---

<ul class="post-list">
{% for post in site.categories.design limit:5 %} 
  <li><article><a href="{{ site.url }}{{ post.url }}">{{ post.title }} <span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time></span></a></article></li>
{% endfor %}
</ul>
