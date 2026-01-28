---
layout: default
title: Nollstead Studio
---

<!-- Top navigation -->  
<nav class="top-links">
  <a href="{{ '/' | relative_url }}">Home</a>
  <a href="{{ '/about/' | relative_url }}">About</a>
</nav>

# Projects
<div class="card-grid">

{%- comment -%}
Build two lists:
1) featured projects (featured == true), sorted by weight
2) regular projects (no featured flag), sorted by weight
Then concatenate so featured items come first.
Optionally: secondary sort by title for deterministic ordering.
{%- endcomment -%}

{%- assign featured = site.projects
  | where_exp: "p", "p.featured == true"
  | sort: "title"
  | sort: "weight"
-%}

{%- assign regular = site.projects
  | where_exp: "p", "p.featured != true"
  | sort: "title"
  | sort: "weight"
-%}

{%- assign items = featured | concat: regular -%}

{%- for p in items -%}
  <article class="card">
    <a class="card-link" href="{{ p.url | relative_url }}" aria-label="{{ p.title }}"></a>

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
