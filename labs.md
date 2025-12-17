---
layout: default
title: Labs
permalink: /labs/
---

<h1>Labs & Write-ups</h1>
<p>
Hands-on labs, SIEM analysis, detection logic, and Blue Team investigations.
</p>

<hr>

<ul>
{% for post in site.posts %}
  {% if post.categories contains "lab" %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a><br>
      <small>{{ post.date | date: "%d %B %Y" }}</small>
    </li>
    <br>
  {% endif %}
{% endfor %}
</ul>
