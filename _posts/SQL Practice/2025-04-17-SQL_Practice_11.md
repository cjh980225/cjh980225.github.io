---
title: "[Programmers_SQL] 자동차 대여 기록 별 대여 금액 구하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 4️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/151141)

## TABLE 설명

◾ CAR_RENTAL_COMPANY_CAR 

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`CAR_ID`|INTEGER|FALSE|자동차 ID|
|`CAR_TYPE`|VARCHAR(255)|FALSE|자동차 종류|
|`DAILY_FEE`|INTEGER|FALSE|일일 대여 요금(원)|
|`OPTIONS`|VARCHAR(255)|FALSE|자동차 옵션 리스트|

▪ 자동차 종류 = ['세단', 'SUV', '승합차', '트럭', '리무진']   
▪ 자동차 옵션 리스트는 콤마(',')로 구분된 키워드 리스트로 되어있습니다.    
▪ 자동차 옵션 키워드 종류 = ['주차감지센서', '스마트키', '네비게이션', '통풍시트', '열선시트', '후방카메라', '가죽시트']

◾ CAR_RENTAL_COMPANY_RENTAL_HISTORY 

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`HISTORY_ID`|INTEGER|FALSE|자동차 대여 기록 ID|
|`CAR_ID`|INTEGER|FALSE|자동차 ID|
|`START_DATE`|DATE|FALSE|대여 시작일|
|`END_DATE`|DATE|FALSE|대여 종료일|

◾ CAR_RENTAL_COMPANY_DISCOUNT_PLAN  

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`PLAN_ID`|INTEGER|FALSE|요금 할인 정책 ID|
|`CAR_TYPE`|VARCHAR(255)|FALSE|자동차 종류|
|`DURATION_TYPE`|VARCHAR(255)|FALSE|대여 기간 종류|
|`DISCOUNT_RATE`|INTEGER|FALSE|할인율(%)|

▪ 할인율이 적용되는 기간 종류

    ▫ '7일 이상' (대여 기간이 7일 이상 30일 미만인 경우)
    ▫ '30일 이상' (대여 기간이 30일 이상 90일 미만인 경우)
    ▫ '90일 이상' (대여 기간이 90일 이상인 경우우)
    ▫ 대여 기간이 7일 미만일 경우 할인정책이 없습니다. 

## 문제 설명

◾ CAR_RENTAL_COMPANY_CAR 테이블과 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블과 CAR_RENTAL_COMPANY_DISCOUNT_PLAN 테이블에서 자동차 종류가 '트럭'인 자동차의 대여 기록에 대해서 대여 기록 별로 대여 금액 (컬럼명 : `FEE`)를 구하여 대여 기록 ID와 대여 금액 리스트를 출력하는 SQL문을 작성해주세요. 

◾ 결과는 대여 금액을 기준으로 내림차순 정렬하고, 대여 금액이 같은 경우 대여 기록 ID를 기준으로 내림차순 정렬해주세요. 

## solution.sql
    SELECT HISTORY_ID, 
        ROUND(DAILY_FEE*(DATEDIFF(END_DATE, START_DATE)+1)
        *(100-IF(DISCOUNT_RATE IS NULL, 0, DISCOUNT_RATE))/100) AS FEE
    FROM CAR_RENTAL_COMPANY_CAR AS A 
    JOIN CAR_RENTAL_COMPANY_RENTAL_HISTORY AS B 
    ON A.CAR_ID = B.CAR_ID
    LEFT OUTER JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN AS C
    ON A.CAR_TYPE = C.CAR_TYPE
    AND C.DURATION_TYPE = (CASE WHEN DATEDIFF(END_DATE,START_DATE)+1>='90' THEN '90일 이상'
                                WHEN DATEDIFF(END_DATE,START_DATE)+1>='30' THEN '30일 이상'
                                WHEN DATEDIFF(END_DATE,START_DATE)+1>='7' THEN '7일 이상'
                            ELSE NULL END)
    WHERE A.CAR_TYPE = '트럭'
    ORDER BY FEE DESC, B.HISTORY_ID DESC;

```
◾ IF(DISCOUNT_RATE IS NULL, 0, DISCOUNT_RATE)
    ▪ DISCOUNT_RATE가 NULL이라면, 0% 할인을 받고, 이외의 것에는 그것에 맞는 할인율을 적용시키는 구문입니다. 
◾ JOIN을 통해 세 테이블을 합치고, CASE WHEN을 사용하여 DATEDIFF로 계산한 값에 맞는 DURATION_TYPE을 적용합니다. 
    ▪ A와 B는 CAR_ID, A와 C는 CAR_TYPE으로 JOIN 합니다. 
    ▪ A와 B는 JOIN, 공통된 ID만 결과에 나옵니다. 
    ▪ A와 C는 LEFT OUTER JOIN, A TABLE 기준으로 다 나오고, C에 없는 것은 전부 NULL 처리합니다. 
    ▪ 그러므로 IF절을 통해 NULL은 0% 할인률을 적용할 수 있습니다. 
◾ DATEDIFF(END_DATE, START_DATE)에서 +1을 하는 이유는 날짜를 계산할 때 1을 더해주는 것과 같은 이치입니다. 
◾ CAR_TYPE 이 '트럭'인 것을 출력하고 정렬 기준을 맞춥니다.
```