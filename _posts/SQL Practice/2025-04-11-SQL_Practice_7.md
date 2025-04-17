---
title: "[Programmers_SQL] 자동차 평균 대여 기간 구하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 2️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/157342)

## TABLE 설명

◾ CAR_RENTAL_COMPANY_RENTAL_HISTORY

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`HISTORY_ID`|INTEGER|FALSE|자동차 대여 기록 ID|
|`CAR_ID`|INTEGER|FALSE|자동차 ID|
|`START_DATE`|DATE|FALSE|대여 시작일|
|`END_DATE`|DATE|FALSE|대여 종료일|

## 문제 설명

◾ CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 평균 대여 기간이 7일 이상인 자동차들의 자동차 ID와 평균 대여 기간 (컬럼명 : `AVERAGE_DURATION`) 리스트를 출력하는 SQL문을 작성해주세요.

◾ 평균 대여 기간은 소수점 두번째 자리에서 반올림 합니다. 

◾ 결과는 평균 대여 기간을 기준으로 내림차순 정렬합니다.

◾ 만약 평균 대여 기간이 같다면, 자동차 ID를 기준으로 내림차순 정렬합니다. 

## solution.sql
    SELECT CAR_ID, ROUND(AVG(DATEDIFF(END_DATE, START_DATE)+1), 1) AS AVERAGE_DURATION
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
    GROUP BY CAR_ID
    HAVING AVERAGE_DURATION >= 7
    ORDER BY AVERAGE_DURATION DESC, CAR_ID DESC;

```
◾ DATEDIFF(END_DATE, START_DATE) + 1 
    ▪ 첫번째 인자에서 두번째 인자를 빼줍니다. 
    ▪ 날짜 계산이기 때문에 1을 더해줘야 기간이 나옵니다. 
◾ AVG()
    ▪ 괄호안의 값의 평균을 계산합니다. 
◾ ROUND( , 1)
    ▪ 소수점 두번째 자리에서 반올림하여 계산합니다. 
◾ GROUP BY 를 CAR_ID로 묶어주고, GROUP BY와 단짝인 HAVING을 통해 평균 대여 기간이 7일 이상한 자동차를 출력합니다. 
◾ 정렬 순서를 참고하여 AVERAGE_DURATION, CAR_ID 순으로 내림차순 DESC 합니다. 
```