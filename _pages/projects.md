---
layout: archive
title: "Artificial Intelligence projects"
permalink: /AI-projects/
author_profile: true  
breadcrumbs: true
---

4 more case studies are to be added, names of which are listed -  

Facebook friend recommendation using graph mining | Human activity recognition | AI generated music | Self-driving car


{% include base_path %}

<div class="grid__wrapper">
  {% for post in site.posts %}
    {% if post.categories contains 'Project' %}
      {% include archive-single.html type="grid" %}
    {% endif %}
  {% endfor %}
</div>
