---
title: "Feature Engineering (특징 엔지니어링 & 데이터 전처리)"
layout: archive
permalink: categories/Feature_Eingineering
author_profile: true
types: posts
sidebar_main: true
---

{% assign posts = site.categories['Feature_Eingineering']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}