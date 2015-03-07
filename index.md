---
layout: page
title: RC's Blog
---

<dl class="posts">
  {% for post in site.posts %}
    <dd class = 'post-date'>{{ post.date | date_to_string }}</dd>
    <dt class = 'post-title'><a href = "{{ site.JB.BASE_PATH }}{{ post.url }}">{{ post.title }}</a></dt>
    <dd class = 'post-excerpt'>{{ post.excerpt }}</dd>
    <dd class = 'post-link'><a href = "{{ site.JB.BASE_PATH }}{{ post.url }}">Read More &rarr;</a></dd>
  {% endfor %}
</dl>
