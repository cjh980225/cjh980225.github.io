---
title: "Model Training & Tuning (모델 학습 & 튜닝)"
layout: archive
permalink: categories/Model_Training&Tuning
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ 머신러닝 모델을 학습시키고 성능을 최적화하는 방법을 다룹니다.   
  ▪️ 하이퍼파라미터 튜닝, 모델 평가 지표, 과적합 방지, 교차 검증

{% assign posts = site.categories['Model_Training&Tuning']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}