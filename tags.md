---
layout: default
title: Tags
permalink: /tags/
---

<h1>Tags</h1>

<ul>
{% assign tags = site.tags | sort %}
{% for tag in tags %}
  <li>
    <strong>{{ tag[0] }}</strong> ({{ tag[1].size }})
  </li>
{% endfor %}
</ul>
