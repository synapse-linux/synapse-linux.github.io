---
layout: default
title: Blog
permalink: /en/blog/
lang: en
---

# Blog

{% for post in site.posts %}
{% if post.lang == 'en' or post.lang == nil %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.date | date: '%B %d, %Y' }}
{% endif %}
{% endfor %}
