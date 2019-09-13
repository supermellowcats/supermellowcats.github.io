---
layout: collection
title: Projects
search: true
show_excerpts: false
entries_layout: list
---

<ul>
  {% for post in site.posts %}
    {% if post.categories[0] == 'Projects' %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      {{ post.excerpt }}
    </li>
    {% endif %}
  {% endfor %}
</ul>
