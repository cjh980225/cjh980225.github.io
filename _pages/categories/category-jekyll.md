---
title: "개발 블로그"
layout: archive
permalink: categories/jekyll
author_profile: true
types: posts
sidebar_main: true
---

{% assign posts = site.categories['jekyll']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}