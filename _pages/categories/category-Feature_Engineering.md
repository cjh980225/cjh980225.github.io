---
title: "Feature Engineering (특징 엔지니어링 & 데이터 전처리)"
layout: archive
permalink: categories/Feature_Engineering
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ 머신러닝 모델의 성능을 높이기 위한 데이터 변환 기법을 다룹니다.   
  ▪️ 데이터 전처리, 피처 스케일링, 결측치 처리, 변수 변환(Encoding), 차원 축소

{% assign posts = site.categories['Feature_Engineering']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}