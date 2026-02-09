---
layout: default
title: Nollstead Studio 
breadcrumb:
  - { title: "Home" }   # current page: no url
---

# Welcome

Nollstead Studio is a community for makers, engineers, and technology enthusiasts who enjoy projects that blend multiple domains—embedded systems development, hardware and PCB design, and wireless technologies. The work here often draws on platforms like Arduino, ESP32, and STM32 as examples, but the focus is on the ideas, techniques, and problem‑solving that bring complex systems together. Whether you're exploring new concepts, diving deep into technical challenges, or sharing your own discoveries, this space celebrates curiosity, innovation, and the craft of building things that bridge the digital and physical worlds.

## Featured Projects

<div class="card-grid">

{%- assign items = site.projects
  | where_exp: "p", "p.featured == true"
  | sort: "title"      /* secondary key */
  | sort: "weight"     /* primary key   */
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