---
layout: default
title: Nollstead Studio
---

<!-- Simple top nav (non-project links). Adjust as needed. -->
<nav class="top-links">
  /Home</a>
  /about/About</a>
</nav>

# Projects

<!-- Temporary diagnostic: should show a number (remove later) -->
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
