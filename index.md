---
layout: page
title: Hello World!
tagline: Supporting tagline
---
{% include JB/setup %}



##Posts List

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## Links
[Coursera][coursera]   [GitHub][github]  [Mindhacks][mindhacks]





[coursera]: https://www.coursera.org
[github]: https://www.github.com 
[mindhacks]: https://mindhacks.cn



