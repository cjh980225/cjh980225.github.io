---
title: "EDA & Visualization (탐색적 데이터 분석 & 시각화)"
layout: archive
permalink: categories/EDA&Visualization
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ 데이터를 탐색하고 시각화하여 패턴과 인사이트를 찾는 과정입니다.   
  ▪️ 데이터 정리, 분포 확인, 상관관계 분석, Matplotlib/Seaborn/Plotly 등을 활용한 시각화 방법

{% assign posts = site.categories['EDA&Visualization']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}