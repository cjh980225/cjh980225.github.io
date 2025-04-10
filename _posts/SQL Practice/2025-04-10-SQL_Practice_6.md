---
title: "[Programmers_SQL] 특정 옵션이 포함된 자동차 리스트 구하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/157343)

## TABLE 설명

◾ CAR_RENTAL_COMPANY_CAR

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`CAR_ID`|INTEGER|FALSE|자동차 ID|
|`CAR_TYPE`|VARCHAR(255)|FALSE|자동차 종류|
|`DAILY_FEE`|INTEGER|FALSE|일일 대여 요금(원)|
|`OPTIONS`|VARCHAR(255)|FALSE|자동차 옵션 리스트|

## 문제 설명

◾ CAR_REATAL_COMPANY_CAR 테이블에서 '네비게이션' 옵션이 포함된 자동차 리스트를 출력하는 SQL문을 작성해주세요. 

◾ 결과는 자동차 ID를 기준으로 내림차순 정렬해주세요. 

## solution.sql
    SELECT *
    FROM CAR_RENTAL_COMPANY_CAR
    WHERE OPTIONS LIKE "%네비게이션%"
    ORDER BY CAR_ID DESC;

```
◾ 모든 COLUMN을 출력하는 *을 SELECT 합니다. 
◾ OPTIONS에서 네비게이션이 들어있는 모든 것을 출력해야 하므로 LIKE "%네비게이션%"을 사용했습니다.
    ▪ %네비게이션%을 통해 앞, 뒤에 다른 문자열이 같이 있더라도 네비게이션이라는 단어가 있으면 모두 출력할 수 있습니다. 
◾ CAR_ID를 통해 내림차순을 하기 위해 DESC를 추가합니다.    
```