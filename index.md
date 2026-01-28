---
layout: default
title: Nollstead Studio
---

<!-- Top navigation: add more non-project pages as you create them -->
<nav class="top-links">
  {{ "/" | relative_url }}Home</a>
  {{ "/about/" | relative_url }}About</a>
</nav>

# Projects

<!-- If this prints a number, your collection is wired correctly. Remove later. -->
<p class="muted">Projects count: {{ site.projects | size }}</p>

<div class="card-grid">
{%- assign items = site.projects | sort: "weight" -%}
{%- for p in items -%}
  <article class="card">
    {{ p.url | relative_url }}</a>

    {% if p.image %}
    <div class="card-media" style="background-image:url('{{ p.image | relative_url }}');"></div>
    {% endif %}

    <div class="card-body">
      <h3 class="card-title">{{ p.title }}</h3>
      {% if p.description %}<p class="card-desc">{{ p.description }}</p>{% endif %}
      {% if p.tags %}
      <div class="card-tags">
        {%- for t in p.tags -%}<span>{{ t }}</span>{%- endfor -%}
      </div>
      {% endif %}
    </div>
  </article>
{%- endfor -%}
</div>
