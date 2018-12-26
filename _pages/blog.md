---
layout: archive
title: "Frequent activities"
permalink: /activity/
author_profile: true  
---

For complete blogs refer my [Medium](https://medium.com/@vivekjaglan) channel.


{% include base_path %}

<div class="grid__wrapper">
  {% for post in site.posts %}
    {% if post.categories contains 'Blogs' %}
      {% include archive-single.html type="grid" %}
    {% endif %}
  {% endfor %}
</div>

