---
layout: default
title: Blog
permalink: /blog/
---

<p>
For hands-on technical content, visit the
<a href="/labs/">Labs & Write-ups</a> section.
</p>

<hr>

<h1>Blog</h1>
<p>
All posts related to cybersecurity, Blue Team operations,
SOC analysis, SIEM & EDR technologies, and lab work.
</p>

<hr>

<ul>
  {% for post in site.posts %}
    <li>
  <strong>
    <a href="{{ post.url }}">{{ post.title }}</a>
  </strong><br>
  <small>{{ post.date | date: "%d %B %Y" }}</small>
</li>
    <br>
  {% endfor %}
</ul>
