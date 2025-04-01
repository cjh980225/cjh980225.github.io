---
title: "Books & Courses (책 & 강의 정리)"
layout: archive
permalink: categories/Books&Courses
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ 데이터 분석 관련 책과 강의를 정리합니다.   
  ▪️ 책 요약, 강의 필기, 학습 내용 정리

{% assign posts = site.categories['Books&Courses']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}