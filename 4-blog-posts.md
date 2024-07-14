---
layout: page
title: Blog Posts
permalink: /blog-posts
---

{% for post in site.posts %}

### [{{ post.title }}]({{ post.url }}) <span style="float:right">_{{ post.date | date: "%b %-d, %Y" }}_</span>

{{ post.excerpt }}

{% endfor %}
