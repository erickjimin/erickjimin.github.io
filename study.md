---
layout: single
title: "Study Posts"
permalink: /categories/study/
author_profile: true
sidebar:
  nav: "main"
---

{% assign study_posts = site.posts | where_exp: "post", "post.categories contains 'study'" %}

{% for post in study_posts %}
  {% include archive-single.html type="post" %}
{% endfor %}
