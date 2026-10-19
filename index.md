---
layout: default
title: Home
---
<div class="home">
<h1>My Notes</h1>
{%- assign notes = site.pages | where_exp: "p", "p.path contains 'content/'" | sort: "path" -%}
{%- assign last_group = "" -%}
{%- assign last_sub = "" -%}
{%- for n in notes -%}
{%- assign parts = n.path | split: "/" -%}
{%- if parts.size > 2 -%}
{%- assign group = parts[1] -%}
{%- assign sub = "" -%}
{%- if parts.size > 3 -%}{%- assign sub_index = parts.size | minus: 2 -%}{%- assign sub = parts[sub_index] -%}{%- endif -%}
{%- if sub == "SQL" -%}{%- assign group = "SQL" -%}{%- assign sub = "" -%}{%- endif -%}{%- if sub == group -%}{%- assign sub = "" -%}{%- endif -%}
{%- if group != last_group -%}
{%- if last_group != "" -%}</ul>{%- endif -%}
<h2>{% if group.size <= 4 %}{{ group | upcase }}{% else %}{{ group | capitalize }}{% endif %}</h2>
{%- assign last_group = group -%}
{%- assign last_sub = "__none__" -%}
{%- endif -%}
{%- if sub != last_sub -%}
{%- if last_sub != "__none__" -%}</ul>{%- endif -%}
{%- if sub != "" -%}<h3 class="home-sub">{{ sub }}</h3>{%- endif -%}
<ul class="home-list">
{%- assign last_sub = sub -%}
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
