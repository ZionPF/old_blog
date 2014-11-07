---
layout: page
title: Out of your confort zone...
---
{% include JB/setup %}

#随心记，随手记

##有好的coding感悟记，有好吃的记，玩爽了偶尔也会记一发

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



