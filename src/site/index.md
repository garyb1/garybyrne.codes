---
title: Gary Byrne
subtitle: A frontend engineer based in South-East Ireland.
layout: layouts/base.njk
---

## Writing

Here is some recent articles by Gary.

<ul class="listing">
{%- for page in collections.post -%}
  <li>
    <a href="{{ page.url }}">{{ page.data.title }}</a> -
    <time datetime="{{ page.date }}">{{ page.date | dateDisplay("LLLL d, y") }}</time>
  </li>
{%- endfor -%}
</ul>
