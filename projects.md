---
layout: default
title: Projects 
permalink: /projects/   
breadcrumb:
  - { title: "Home", url: "/" }
  - { title: "Projects" }
---

# All Projects

Lorem ipsum dolor sit amet consectetur adipiscing elit. Quisque faucibus ex sapien vitae pellentesque sem placerat. In id cursus mi pretium tellus duis convallis. Tempus leo eu aenean sed diam urna tempor. Pulvinar vivamus fringilla lacus nec metus bibendum egestas. Iaculis massa nisl malesuada lacinia integer nunc posuere. Ut hendrerit semper vel class aptent taciti sociosqu. Ad litora torquent per conubia nostra inceptos himenaeos.

<div class="card-grid">

{%- assign items = site.projects
  | sort: "title"      /* secondary key (applied first) */
  | sort: "weight"     /* primary key  (applied last)  */
-%}

{%- for p in items -%}
  {%- assign parts = p.path | split: '/' -%}
  {%- if parts.size == 2 -%}

  <article class="card">
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

  {%- endif -%}
{%- endfor -%}
</div>