---
layout: default
title: Categories
permalink: /categories/
---

<h1>Categories</h1>

<ul>
{% assign categories = site.categories | sort %}
{% for category in categories %}
  <li>
    <strong>{{ category[0] }}</strong> ({{ category[1].size }})
  </li>
{% endfor %}
</ul>
