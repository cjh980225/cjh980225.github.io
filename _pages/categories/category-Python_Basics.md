---
title: "Python Basics (기본 문법 & 라이브러리)"
layout: archive
permalink: categories/Python_Basics
author_profile: true
types: posts
sidebar_main: true
---

{% assign posts = site.categories['Python_Basics']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}