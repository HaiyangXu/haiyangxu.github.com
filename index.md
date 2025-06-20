---
layout: page
title: Haiyang Xu	
tagline: A Computer Vision Blog
---
{% include JB/setup %}

{% for post in site.posts limit:5 %}
<article class="post-preview">
  <h2 class="post-title">
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </h2>
  
  <div class="post-meta">
    <time datetime="{{ post.date | date_to_xmlschema }}">
      {{ post.date | date: "%B %-d, %Y" }}
    </time>
  </div>

  <div class="post-excerpt">
    {{ post.excerpt }}
    <a href="{{ post.url | relative_url }}" class="read-more">Read more...</a>
  </div>
</article>
{% endfor %}

## Links
- [Coursera](https://www.coursera.org)
- [GitHub](https://github.com/{{ site.author.github }})
- [Mindhacks](http://mindhacks.cn)