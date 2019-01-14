---
layout: archive
title: "Artificial Intelligence projects"
permalink: /AI-projects/
author_profile: true  
breadcrumbs: true
---

11 more case studies are to be added, names of which are listed -  

NYC Taxi demand prediction (Kaggle) | Microsoft malware detection (Kaggle) | Facebook friend recommendation using graph mining | Netflix movie recommendation system | Human activity recognition | AI generated music | Self-driving car | Personalized cancer diagnosis.


{% include base_path %}

<div class="grid__wrapper">
  {% for post in site.posts %}
    {% if post.categories contains 'Project' %}
      {% include archive-single.html type="grid" %}
    {% endif %}
  {% endfor %}
</div>
