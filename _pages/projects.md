---
layout: page
title: Projects
permalink: /projects/
description: 
nav: true
nav_order: 3
display_categories: [work, fun]
horizontal: false
---

<!-- pages/projects.md -->
<div class="projects">
  {% assign sorted_projects = site.projects | sort: "importance" %}
  
  <ul class="list-unstyled">
    {% for project in sorted_projects %}
      {% include project_entry.liquid project=project %}
    {% endfor %}
  </ul>
</div>
