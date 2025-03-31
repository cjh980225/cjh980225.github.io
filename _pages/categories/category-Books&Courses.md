---
title: "Books & Courses (책 & 강의 정리)"
layout: archive
permalink: categories/Books&Courses
author_profile: true
types: posts
sidebar_main: true
---

{% assign posts = site.categories['Books&Courses']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}