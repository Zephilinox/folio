---
layout: default
title: tags
permalink: /category/
---

<div class="header-bar">
  <h1>*folio</h1>
  <h2>simple whitespace theme</h2>
  <br/>
  <hr>
  <br/>
</div>


<ul class="post-list">
    {% for category in site.categories %}
    ## {category} ##
      <li>
      {% for post in site.categories[category.first] %}
            <h2><a class="post-title" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h2>
            <p class="post-meta">{{ post.date | date: '%B %-d, %Y — %H:%M' }}</p>
            <p>{{ post.description }}</p>
            <br/>
            <hr/>
        {% endfor %}
      </li>
      
    {% endfor %}
</ul>


<div class="container mtb">
    <div class="row">
        <div class="col-lg-8">

        <! -- BLOG POSTS LIST -->
        {% for category in site.categories %}

                <a class="anchor" id="{{ category.first }}"></a><h4>{{ category.first }}</h4>
                    <div class="hline"></div>
                    <ul class="popular-posts">
                        {% for post in site.categories[category.first] %}

                            <li>
                                <a href="{{ post.url  | prepend: site.baseurl }}"><img src="{{ "/assets/img/blog/thumbs/" | prepend: site.baseurl }}{{ post.thumb }}" alt="Popular Post"></a>
                                <p><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></p>
                                <em>Posted on {{ post.date | date_to_string }}</em>
                            </li>

            {% endfor %}
                                                </ul>


        {% endfor %}
        </div><! --/col-lg-8 -->

    </div><! --/row -->
</div><! --/container -->
