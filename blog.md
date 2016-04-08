---
layout: page
title: Blog
---

{% for post in site.categories.projects %}
  <h1>{{ post.title }}</h1>
  {{ post.content }}
{% endfor %}

{% for post in site.posts %}
  * {{ post.date | date_to_string }} {% include reading_time.html %} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}
