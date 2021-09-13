---
layout: collection
title: Projects
search: true
show_excerpts: false
entries_layout: list
---

<ul>
  {% for post in site.categories.Projects %}
    <li>
      <h3 id="page-title" class="page-title p-name">
        <a href="{{ post.url }}">{{ post.title }}</a>
      </h3>
      <span class="entry-date" style="font-weight:bold;float:right"><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time></span>
      <br>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
