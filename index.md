---
layout: default
title: Home
---

# My Obsidian Notes

Welcome to my public knowledge base.

## All Notes

{% assign notes = site.pages | where_exp: "p", "p.path contains 'content/'" | sort: "name" %}
{% for n in notes %}
- [{{ n.name | remove: ".md" }}]({{ n.url | relative_url }})
{% endfor %}
