
---
layout: default
title: Home
---

# My Obsidian Notes

Welcome to my public knowledge base.

## All notes

<ul>
{% assign notes = site.static_files | where_exp: "file", "file.path contains '/content/'" | sort: "name" %}
{% for note in notes %}
{% assign extension = note.extname | downcase %}
{% if extension == ".md" %}
<li><a href="{{ note.path | relative_url | replace: '.md', '.html' }}">{{ note.name | replace: '.md', '' | escape }}</a></li>
{% endif %}
{% endfor %}
</ul>
