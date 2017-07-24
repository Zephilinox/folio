---
layout: default
title: tags
permalink: /tags/
---

<div class="header-bar">
  <h1>categories</h1>
  <br/>
</div>


<ul class="post-list">
    {% for tag in site.tags %}
    <h1><p class="post-title">{{ tag.first }}</p></h1>
      <hr>
      <br/>
      <li>
      {% for post in site.tags[tags.first] %}
            <h2><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h2>
            <p class="post-meta">{{ post.date | date: '%B %-d, %Y — %H:%M' }}</p>
            <p>{{ post.description }}</p>
            <br/>
        {% endfor %}
      </li>
      
    {% endfor %}
</ul>
