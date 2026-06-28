---
layout: default
title: Blog
permalink: /it/blog/
lang: it
---

# Blog

{% for post in site.posts %}
{% if post.lang == 'it' %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.date | date: '%B %d, %Y' }}
{% endif %}
{% endfor %}
