---
layout: archive
title: "Artificial Intelligence projects"
permalink: /AI-projects/
author_profile: true  
breadcrumbs: true
---


{% include base_path %}

<div class="grid__wrapper">
  {% for post in site.posts %}
    {% if post.categories contains 'Project' %}
      {% include archive-single.html type="grid" %}
    {% endif %}
  {% endfor %}
</div>
