---
title: "Personal Projects (개인 프로젝트)"
layout: archive
permalink: categories/Personal_Projects
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ 개인적으로 진행한 데이터 분석 및 머신러닝 프로젝트를 공유합니다.   
  ▪️ 데이터 수집부터 분석, 모델링, 시각화까지 프로젝트 전체 과정 정리

{% assign posts = site.categories['Personal_Projects']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}