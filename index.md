---
layout: home
permalink: /
---

![Landingpage](/assets/images/landingpage.png)

Welcome to my personal website! On this site, you can find information about me, my projects, and my blog posts. This is a place where I share my thoughts, ideas, and projects with the world. I hope you find something interesting here!

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
