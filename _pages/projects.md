---
layout: page
title: blog 
permalink: /blog/
description:
nav: true
nav_order: 2
display_categories: [tools, stories]
horizontal: false
masonry: true
---

<!-- pages/projects.md -->
<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {% for category in page.display_categories %}
  <div id="{{ category }}" style="text-align: left; margin-bottom: 2rem;">
    <h2 class="category" style="text-align: left; margin-bottom: 0.25rem;">{{ category }}</h2>
    {% if category == "tools" %}
    <p style="font-size: 1.1rem; color: #666; margin-top: 0;">systems i use to think, build, and live more deliberately</p>
    {% elsif category == "stories" %}
    <p style="font-size: 1.1rem; color: #666; margin-top: 0;">narratives that shape how i see the world</p>
    {% endif %}
  </div>
  {% assign categorized_projects = site.projects | where: "category", category %}
  {% assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display projects without categories -->

{% assign sorted_projects = site.projects | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
