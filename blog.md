---
layout: page
title: Blog
---

{% for post in site.categories.projects %}
  <h1>{{ post.title }}</h1>
  {{ post.content }}
{% endfor %}

{% for post in site.posts %}
{% capture words %}
  {{ post.content | number_of_words | minus: 180 }}
{% endcapture %}
  * {{ post.date | date_to_string }} ({% unless words contains "-" %}
  {{ words | plus: 180 | divided_by: 180 | append: " _minutes to read_" }}
{% endunless %}) &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}
