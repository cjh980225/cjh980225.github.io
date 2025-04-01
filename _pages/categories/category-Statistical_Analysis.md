---
title: "Statistical Analysis (통계 분석)"
layout: archive
permalink: categories/Statistical_Analysis
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ 데이터의 특징을 파악하고 통계적 방법으로 분석합니다.   
  ▪️ 가설 검정, 신뢰 구간, 회귀 분석, 정규성 검정, t-검정 등 통계적 기법

{% assign posts = site.categories['Statistical_Analysis']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}