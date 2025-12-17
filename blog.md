---
layout: default
title: Blog
permalink: /blog/
---

<h1>Blog</h1>
<p>
All posts related to cybersecurity, Blue Team operations,
SOC analysis, SIEM & EDR technologies, and lab work.
</p>

<hr>

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a><br>
      <small>{{ post.date | date: "%d %B %Y" }}</small>
    </li>
    <br>
  {% endfor %}
</ul>
