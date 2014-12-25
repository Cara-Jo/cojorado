---
layout: page
title: "Adventures"
date: 
modified:
excerpt:
tags: []
image:
  feature:
---
Sometimes I go on adventures - sometimes they're epic, but mostly they're small.

<ul class="post-list">
{% for post in site.categories.adventures %} 
  <li><article><a href="{{ site.url }}{{ post.url }}">{{ post.title }} <span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time></span></a></article></li>
{% endfor %}
</ul>
