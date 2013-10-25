---
layout: page
title:  Haiyang Xu	
tagline: A Computer Vision Blog
index: hide tilte at the front 
---
{% include JB/setup %}

##Recent Articles
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}"><span>{{ post.date | date_to_string }}</span> &raquo; {{ post.title }}</a>
      <p>{{ post.excerpt }}  <a href="{{ post.url }}">  more...</a> </p>
    </li>
  {% endfor %}
</ul>


## Links
[Coursera][coursera]   [GitHub][github]  [Mindhacks][mindhacks]





[coursera]: https://www.coursera.org
[github]: https://www.github.com 
[mindhacks]: https://mindhacks.cn



