---
layout: default
title: Nollstead Studio
---

<!-- Simple top nav (non-project links). Adjust as needed. -->
<nav class="top-links">
  <a href="/">Home</a>
  <a href="/about/">About</a>
  <!-- Add other topics/pages here -->
</nav>

# Projects

<div class="card-grid">
  {%- assign items = site.projects | sort: "weight" -%}
  {%- for p in items -%}
    <article class="card">
      <a class="card-link" href="{{ p.url | relative_url }}" aria-label="{{ p.title }}"></a>

      {% if p.image %}
      <div class="card-media" style="background-image:url('{{ p.image | relative_url }}');"></div>
      {% endif %}

      <div class="card-body">
        <h3 class="card-title">{{ p.title }}</h3>
        <p class="card-desc">{{ p.description }}</p>
        <div class="card-tags">
          {%- for t in p.tags -%}<span>{{ t }}</span>{%- endfor -%}
        </div>
      </div>
    </article>
  {%- endfor -%}
</div>
