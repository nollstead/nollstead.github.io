---
layout: default
title: Nollstead Studio
---

[Home]({{ '/' | relative_url }})  [About]({{ '/about/' | relative_url }})

# Projects

{%- comment -%}
Only list *top-level* documents from the projects collection as tiles.
"_projects/<file>.md"               → ("p.path" | split: "/") size == 2  → keep
"_projects/<folder>/<file>.md"      → size >= 3                         → exclude
This prevents inner pages (e.g., _projects/usbbuddy/loopback.md) from appearing as tiles.
{%- endcomment -%}
{%- assign top_level = site.projects
  | where_exp: "p", "(p.path | split: '/' | size) == 2"
-%}

{%- comment -%}
Featured first (weight primary, title secondary), then regular (same sort).
Sorting by title first and then by weight is a common Liquid pattern to get
weight as the *primary* key while keeping a deterministic tiebreaker.
{%- endcomment -%}
{%- assign featured = top_level
  | sort: "title"
  | sort: "weight"
  | where_exp: "p", "p.featured == true"
-%}

{ = top_level
  | sort: "title"
  | sort: "weight"
  | where_exp: "p", "p.featured != true"
-%}

{%- assign items = featured | concat: regular -%}

<div class="card-grid">
{%- for p in items -%}
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
{%- endfor -%}
</div>
