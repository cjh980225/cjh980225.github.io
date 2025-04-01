---
title: "SQL Basics (SQL 기본 문법)"
layout: archive
permalink: categories/SQL_Basics
author_profile: true
types: posts
sidebar_main: true
---

{: .notice--info .text-left}
  ✔ SQL의 기본 문법과 개념을 익힙니다.   
  ▪️ SELECT, INSERT, UPDATE, DELETE, JOIN, GROUP BY, 서브쿼리

{% assign posts = site.categories['SQL_Basics']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}