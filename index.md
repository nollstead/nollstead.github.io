---
layout: default
title: Nollstead Studio 
---

<!-- Simple top nav -->
<nav class="top-links">
  <a href="{{ '/' | relative_url }}">Home</a>
  <a href="{{ '/about/' | relative_url }}">About</a>
</nav>

# Projects

<div class="card-grid">

{%- comment -%}
Build two lists:
1. Featured projects (featured == true), ordered by weight (primary) then title.
2. Non-featured projects, ordered the same way.
Then combine them so featured items appear first.
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
    <!-- Full-card invisible link (correctly formed) -->
    <a class="card-link" href="{{ p.url | relative_url }}" aria-label="{{ p.title }}"></a>

    {% if p.image %}
    <div class="card-media" style="background-image:url('{{ p.image }}');"></div>
    {% endif %}

    <div class="card-body">
      <h3 class="card-title">{{ p.title }}</h3>

      {% if p.description %}
      <p class="card-desc">{{ p.description }}</p>
      {% endif %}

      {% if p.tags %}
      <div class="card-tags">
        {%- for t in p.tags -%}
          <span>{{ t }}</span>
        {%- endfor -%}
      </div>
      {% endif %}
    </div>
  </article>
{%- endfor -%}
</div>
