---
layout: default
title: Home
---

# Welcome to the DevOps Dictionary

This is the homepage of the DevOps Dictionary.

Currently in development and coming soon!

<img src="assets/images/Devops-toolchain.svg" alt="DevOps Toolchain" width="600" height="400">


<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>