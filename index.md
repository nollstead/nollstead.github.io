---
layout: default
title: Nollstead Studio
---

{{ '/' | relative_url }}  {{ '/about/' | relative_url }}

# Projects

{%- comment -%}
Strategy (Liquid-compatible across GitHub Pages):

1) Sort the entire collection by title, then by weight
   (double-sort pattern makes weight the primary key with a deterministic tiebreaker).
2) Render featured first, then regular, using two passes.
3) In each pass, include ONLY top-level documents:
     "_projects/<file>.md" → p.path split size == 2  → keep
     "_projects/<folder>/<file>.md" → size >= 3      → exclude
{%- endcomment -%}

{%- assign sorted = site.projects | sort: "title" | sort: "weight" -%}

<div class="card-grid">

  {%- comment -%} PASS 1: featured top-level items {%- endcomment -%}
  {%- for p in sorted -%}
    {%- assign parts = p.path | split: '/' -%}
    {%- if parts.size == 2 and p.featured == true -%}
      <article class="card">
        {{ p.url | relative_url }}</a>

        {%- if p.image -%}
          <div class="card-media" style="background-image:url('{{ p.image }}');"></div>
        {%- endif -%}

        <div class="card-body">
          <h3 class="card-title">{{ p.title }}</h3>
          {%- if p.description -%}
            <p class="card-desc">{{ p.description }}</p>
          {%- endif -%}
          {%- if p.tags -%}
            <div class="card-tags">
              {%- for t in p.tags -%}<span>{{ t }}</span>{%- endfor -%}
            </div>
          {%- endif -%}
        </div>
      </article>
    {%- endif -%}
  {%- endfor -%}

  {%- comment -%} PASS 2: regular (non-featured) top-level items {%- endcomment -%}
  {%- for p in sorted -%}
    {%- assign parts = p.path | split: '/' -%}
    {%- if parts.size == 2 and p.featured != true -%}
      <article class="card">
        {{ p.url | relative_url }}</a>

        {%- if p.image -%}
          <div class="card-media" style="background-image:url('{{ p.image }}');"></div>
        {%- endif -%}

        <div class="card-body">
          <h3 class="card-title">{{ p.title }}</h3>
          {%- if p.description -%}
            <p class="card-desc">{{ p.description }}</p>
          {%- endif -%}
          {%- if p.tags -%}
            <div class="card-tags">
              {%- for t in p.tags -%}<span>{{ t }}</span>{%- endfor -%}
            </div>
          {%- endif -%}
        </div>
      </article>
    {%- endif -%}
  {%- endfor -%}

</div>
