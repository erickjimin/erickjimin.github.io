---
layout: single
title: "Assignment Posts"
permalink: /categories/assignment/
author_profile: true
sidebar:
  nav: "main"
---

{% assign assignment_posts = site.posts | where_exp: "post", "post.categories contains 'assignment'" %}

{% for post in assignment_posts %}
  {% include archive-single.html type="post" %}
{% endfor %}
