---
layout: home
excerpt: ""
tags: []
image:
  feature: cojo-craterlakes.jpg
---
<div id="main" role="main"> 
  <div id="index" class="post-list">
    
    <div class="category-post-list">
        <h1 class="green">Archives</h1>
        {% for post in paginator.posts %}
          {% include _home-post.html %}
        {% endfor %}
            </div>
     
  </div><!-- /#index -->
</div><!-- /#main -->
{% if paginator.total_pages > 1 %}
<div class="pagination">
  {% if paginator.previous_page %}
    <a rel="prev" href="{{ paginator.previous_page_path | replace: 'index.html', '/' | prepend: site.baseurl | replace: '//', '/' }}">&laquo; Prev</a>
  {% else %}
    <span>&laquo; Prev</span>
  {% endif %}

  

  {% if paginator.next_page %}
    <a rel="next" href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">Next &raquo;</a>
  {% else %}
    <span>Next &raquo;</span>
  {% endif %}
</div>
{% endif %}

