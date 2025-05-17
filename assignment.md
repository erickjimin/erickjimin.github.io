---
layout: default
title: Assignment Posts
permalink: /categories/assignment/
author_profile: true
---

{% assign assignment_posts = site.posts | where_exp:"post", "post.categories contains 'assignment'" %}

<ul>
  {% for post in assignment_posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span style="font-size: 0.8rem;"> - {{ post.date | date: "%Y-%m-%d" }}</span>
    </li>
  {% endfor %}
</ul>
