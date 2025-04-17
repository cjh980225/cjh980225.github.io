---
title: "[Programmers_SQL] 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 3️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/157340)

## TABLE 설명

◾ CAR_RENTAL_COMPANY_RENTAL_HISTORY 

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`HISTORY_ID`|INTEGER|FALSE|자동차 대여 기록 ID|
|`CAR_ID`|INTEGER|FALSE|자동차 ID|
|`START_DATE`|DATE|FALSE|대여 시작일|
|`END_DATE`|DATE|FALSE|대여 종료일|

## 문제 설명

◾ CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 2022년 10월 16일에 대여 중인 자동차인 경우 '대여중'이라고 표시하고, 대여 중이지 않은 자동차인 경우 '대여 가능'을 표시하는 컬럼(컬럼명 : `AVAILABILITY`)를 추가하여 자동차 ID와 `AVAILABILITY` 리스트를 출력하는 SQL문을 작성해주세요. 

◾ 반납 날짜가 2022년 10월 16일인 경우에도 '대여중'으로 표시해주세요. 

◾ 자동차 ID를 기준으로 내림차순 정렬해주세요. 

## solution.sql
    SELECT CAR_ID, 
        CASE WHEN MAX('2022-10-16' BETWEEN START_DATE AND END_DATE) THEN '대여중'
            ELSE '대여 가능'
            END AS AVAILABILITY 
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
    GROUP BY CAR_ID
    ORDER BY CAR_ID DESC;

```
◾ CASE WHEN 
    ▪ START_DATE와 END_DATE 사이에 2022-10-16이 있다면 '대여중'을 출력합니다. 
    ▪ MAX()가 있는 이유 
        ▫ CAR_ID의 대여 기록이 여러 개일 수 있기 때문에 하나라도 조건을 만족하면 TRUE를 리턴하게 해주는 역할을 합니다.
    ▪ 만약 그렇지 않다면 ELSE '대여 가능'을 출력합니다.
    ▪ END AS ABAILABILITY 를 통해 컬럼명을 지정합니다. 
◾ GROUP BY를 통해 CAR_ID로 묶어줍니다.
◾ CAR_ID로 내림차순 정렬합니다. 
```