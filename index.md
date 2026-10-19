---
layout: default
title: Home
---

# My Obsidian Notes

Welcome to my public knowledge base.

## All Notes

{% assign notes = site.pages | where_exp: "p", "p.path contains 'content/'" | sort: "path" %}
{% assign last_group = "" %}
{% for n in notes %}
{% assign parts = n.path | split: "/" %}
{% if parts.size > 2 %}{% assign group = parts[1] %}{% else %}{% assign group = "Others" %}{% endif %}
{% if group != last_group %}

### {% if group.size <= 4 %}{{ group | upcase }}{% else %}{{ group | capitalize }}{% endif %}

{% assign last_group = group %}
{% endif %}
- [{{ n.name | remove: ".md" }}]({{ n.url | relative_url }})
{% endfor %}
