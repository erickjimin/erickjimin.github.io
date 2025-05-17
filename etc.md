---
layout: single
title: "ETC Posts"
permalink: /categories/etc/
author_profile: true
sidebar:
  nav: "main"
---

{% assign etc_posts = site.posts | where_exp: "post", "post.categories contains 'etc'" %}

{% for post in etc_posts %}
  {% include archive-single.html type="post" %}
{% endfor %}
