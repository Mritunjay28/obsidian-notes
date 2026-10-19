---
layout: default
title: Home
---
<div class="home">
<h1>My Notes</h1>
{%- assign notes = site.pages | where_exp: "p", "p.path contains 'content/'" | sort: "path" -%}
{%- assign last_group = "" -%}
{%- for n in notes -%}
{%- assign parts = n.path | split: "/" -%}
{%- if parts.size > 2 -%}
{%- assign group = parts[1] -%}
{%- if group != last_group -%}
{%- if last_group != "" -%}</ul>{%- endif -%}
<h2>{% if group.size <= 4 %}{{ group | upcase }}{% else %}{{ group | capitalize }}{% endif %}</h2><ul class="home-list">
{%- assign last_group = group -%}
{%- endif -%}
<li><a href="{{ n.url | relative_url }}">{{ n.name | remove: ".md" }}</a></li>
{%- endif -%}
{%- endfor -%}
{%- if last_group != "" -%}</ul>{%- endif -%}
{%- assign other_count = 0 -%}
{%- for n in notes -%}{%- assign parts = n.path | split: "/" -%}{%- if parts.size == 2 -%}{%- assign other_count = other_count | plus: 1 -%}{%- endif -%}{%- endfor -%}
{%- if other_count > 0 -%}
<h2>Others</h2><ul class="home-list">
{%- for n in notes -%}{%- assign parts = n.path | split: "/" -%}{%- if parts.size == 2 -%}<li><a href="{{ n.url | relative_url }}">{{ n.name | remove: ".md" }}</a></li>{%- endif -%}{%- endfor -%}
</ul>
{%- endif -%}
</div>
