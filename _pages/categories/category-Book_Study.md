---
title: "서적 리뷰"
layout: archive
permalink: categories/Book_Study
author_profile: true
types: posts
sidebar_main: true
---

{% assign posts = site.categories['Book_Study']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}