---
layout: collection
title: Projects
search: true
show_excerpts: false
entries_layout: list
---

<ul>
  {% for post in site.categories.Project %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span class="entry-date" style="font-weight:bold;float:right"><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time></span>
      <br>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
