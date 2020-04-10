---
layout: layout.njk
pageTitle: khle.dev
---

## Kevin Le's `coding` journal

{%- assign posts = collections.post | reverse -%}
{% for post in posts %}
<span class="badge secondary">{{ post.date | post.date: '%Y-%m-%d' }}</span>  [{{ post.data.title }}]({{ post.url }})
{% endfor %}