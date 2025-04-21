---
title: "[Programmers_SQL] 대여 횟수가 많은 자동차들의 월별 대여 횟수 구하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 3️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/151139)

## TABLE 설명

◾ CAR_RENTAL_COMPANY_RENTAL_HISTORY 

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`HISTORY_ID`|INTEGER|FALSE|자동차 대여 기록 ID|
|`CAR_ID`|INTEGER|FALSE|자동차 ID|
|`START_DATE`|DATE|FALSE|대여 시작일|
|`END_DATE`|DATE|FALSE|대여 종료일|


## 문제 설명

◾ CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 대여 시작일을 기준으로 2022년 8월부터 2022년 10월까지 총 대여 횟수가 5회 이상인 자동차들에 대해서 해당 기간 동안의 월별 자동차 ID별 총 대여 횟수 (컬럼명 : RECORDS) 리스트를 출력하는 SQL문을 작성해주세요.   

◾ 결과는 월을 기준으로 오름차순 정렬, 월이 같아면 자동차 ID를 기준으로 내림차순 정렬해주세요. 

◾ 특정 월의 총 대여 횟수가 0인 경우에는 결과에서 제외해주세요. 

## solution.sql
    SELECT 
        DATE_FORMAT(START_DATE, '%m') AS MONTH,
        CAR_ID,
        COUNT(*) AS RECORDS
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
    WHERE START_DATE >= '2022-08-01' AND START_DATE < '2022-11-01'
    AND CAR_ID IN (
        SELECT CAR_ID
        FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
        WHERE START_DATE >= '2022-08-01' AND START_DATE < '2022-11-01'
        GROUP BY CAR_ID
        HAVING COUNT(*) >= 5
    )
    GROUP BY MONTH, CAR_ID
    ORDER BY MONTH ASC, CAR_ID DESC;


```
◾ 서브쿼리를 통해 지정된 기간 동안 대여 횟수가 5회 이상인 차량만 CAR_ID에 추가합니다. 
◾ WHERE START_DATE >= '2022-08-01' AND START_DATE < '2022-11-01' 구문이 서브쿼리에서는 CAR_ID 리스트만 뽑는 역할을 합니다. 
◾ 서브쿼리 바깥에서는 결국 실제로 8월 ~ 10월의 월별 대여 횟수를 구하는 구문이 됩니다. 
◾ 이후 GROUP BY 를 사용하여 MONTH, CAR_ID 로 그룹화를 시켜준 이후 ORDER BY로 조건에 맞는 정렬을 수행합니다. 
```
