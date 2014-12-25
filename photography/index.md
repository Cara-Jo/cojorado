---
layout: page-category
title: "Photography"
date: 
modified:
excerpt:
tags: []
image:
  feature: castle-valley.png
---

{% for post in site.categories.photography %} 
  {% if post.image.feature %}
  <div class="image-wrap">
  <img src=
    {% if post.image.feature contains 'http' %}
      "{{ post.image.feature }}"
    {% else %}
      "{{ site.url }}/images/{{ post.image.feature }}"
    {% endif %}
  alt="{{ post.title }} feature image">
  </div><!-- /.image-wrap -->
  {% endif %}
  <div class="post-preview-title">
    <h3><a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></h3>
    <p>{{ post.excerpt | strip_html | truncate: 160 }}</p>
  </div>

{% endfor %}
