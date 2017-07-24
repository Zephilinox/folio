---
layout: default
title: tags
permalink: /category/
---

<div class="header-bar">
  <h1>categories</h1>
  <br/>
</div>


<ul class="post-list">
    {% for category in site.categories %}
    <h1><p class="post-title">{{ category.first }}</p></h1>
      <hr>
      <br/>
      <li>
      {% for post in site.categories[category.first] %}
            <h2><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h2>
            <p class="post-meta">{{ post.date | date: '%B %-d, %Y — %H:%M' }}</p>
            <p>{{ post.description }}</p>
            <br/>
            <hr/>
        {% endfor %}
      </li>
      
    {% endfor %}
</ul>
