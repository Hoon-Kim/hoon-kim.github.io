---
layout: default
title:  Search
---
# {{ page.title }}
<div id="search-searchbar"></div>
<div class="post-list" id="search-hits"></div>
<div class="home">
  {% include algolia.html %}
  <p class="rss-subscribe">subscribe <a href="{{ '/feed.xml' | relative_url }}">via RSS</a></p>
</div>
