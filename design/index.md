---
layout: page-category
title: "Design"
date: 
modified:
excerpt:
tags: []
image:
  feature: flatirons.png
---
{% for post in site.categories.design limit:5 %} 
	{% include _home-post.html %}
{% endfor %}
