---
layout: page
title:  Haiyang Xu	
tagline: A Computer Vision Blog
index: hide tilte at the front 
---
{% include JB/setup %}



{% for post in site.posts %}

 <h1 class="title"> <a class="title-link" href="{{ post.url }}">{{post.title}} </a> </h1>

<div class="date emphnext">{{ post.date | date: "%B %-d, %Y"}}
</div>

<p>{{ post.excerpt }}  <a href="{{ post.url }}">Read more...</a> </p>

{% endfor %}


## Links
[Coursera][coursera]   [GitHub][github]  [Mindhacks][mindhacks]





[coursera]: https://www.coursera.org
[github]: https://www.github.com 
[mindhacks]: https://mindhacks.cn



