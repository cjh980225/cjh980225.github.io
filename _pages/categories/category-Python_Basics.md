---
title: "Python Basics (기본 문법 & 라이브러리)"
layout: archive
permalink: categories/Python_Basics
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ 파이썬 문법과 기본 개념을 정리합니다.   
  ▪️ 변수, 리스트/딕셔너리, 함수, 클래스, 파일 입출력 등 기초 문법

{% assign posts = site.categories['Python_Basics']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}