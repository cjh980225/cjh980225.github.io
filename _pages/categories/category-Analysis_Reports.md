---
title: "Analysis Reports (데이터 분석 리포트)"
layout: archive
permalink: categories/Analysis_Reports
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ 데이터 분석 과정과 결과를 보고서 형태로 정리합니다.   
  ▪️ 통계 분석, 시각화, 기업 및 공공 데이터 사례 연구

{% assign posts = site.categories['Analysis_Reports']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}