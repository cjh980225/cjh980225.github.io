---
title: "SQL Practice (실전 문제 풀이)"
layout: archive
permalink: categories/SQL_Practice
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ 실제 데이터셋을 활용해 SQL을 연습합니다.   
  ▪️ Kaggle 데이터 활용, 실전 SQL 문제 풀이, 효율적인 쿼리 작성법

{% assign posts = site.categories['SQL_Practice']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}