---
title: "Competitions & Challenges (대회 & 챌린지 경험)"
layout: archive
permalink: categories/Competitions&Challenges
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ 데이터 분석 및 머신러닝 대회에서의 경험을 공유합니다.   
  ▪️ Kaggle, 해커톤, 분석 대회, 실전 문제 해결 과정

{% assign posts = site.categories['Competitions&Challenges']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}