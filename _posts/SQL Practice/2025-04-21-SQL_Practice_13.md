---
title: "[Programmers_SQL] 자동차 대여 기록에서 장기/단기 대여 구분하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/151138)

## TABLE 설명

◾ CAR_RENTAL_COMPANY_RENTAL_HISTORY 

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`HISTORY_ID`|INTEGER|FALSE|자동차 대여 기록 ID|
|`CAR_ID`|INTEGER|FALSE|자동차 ID|
|`START_DATE`|DATE|FALSE|대여 시작일|
|`END_DATE`|DATE|FALSE|대여 종료일|


## 문제 설명

◾ CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 대여 시작일이 2022년 9월에 속하는 대여 기록에 대해서 대여 기간이 30일 이상이면 '장기 대여', 그렇지 않으면 '단기 대여'로 표시하는 컬럼 (컬럼명 : `RENT_TYPE`)을 추가하여 대여기록을 출력하는 SQL문을 작성해주세요.

◾ 결과는 대여 기록 ID를 기준으로 내림차순 정렬해주세요. 

## solution.sql
    SELECT
        history_id
        ,car_id
        ,DATE_FORMAT(start_date,"%Y-%m-%d") AS start_date
        ,DATE_FORMAT(end_date,"%Y-%m-%d") AS end_date
        ,CASE
        WHEN DATEDIFF(end_date,start_date)+1 >= 30 THEN '장기 대여'
        ELSE '단기 대여' END AS RENT_TYPE
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
    WHERE start_date >= '2022-09-01 00:00:00' AND start_date < '2022-10-01 00:00:00'
    ORDER BY history_id DESC


```
◾ start_date와 end_date의 date_format을 맞추었습니다 (%Y-%m-%d)
◾ DATEDIFF를 통해 차량의 대여 기간을 계산합니다. 
◾ WHEN 절을 통해 대여 기간의 기간이 30일이 넘는다면 '장기 대여', 그렇지 않다면 '단기 대여'를 출력하는 RENT_TYPE 컬럼을 생성합니다. 
◾ WHERE 절을 통해 START_DATE가 9월에 속하는 대여 기록을 설정합니다. 
◾ ORDER BY를 통해 조건에 맞는 정렬을 수행합니다. 
```
