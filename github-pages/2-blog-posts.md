---
layout: page
title: Blog Posts
permalink: /blog-posts
---

{% for post in site.posts %}

### [{{ post.title }}]({{ post.url }})  <span style="float:right">*{{ post.date | date: "%b %-d, %Y" }}*</span>

{{ post.excerpt }}

{% endfor %}
