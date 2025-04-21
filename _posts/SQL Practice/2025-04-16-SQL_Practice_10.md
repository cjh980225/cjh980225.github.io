---
title: "[Programmers_SQL] 특정 기간동안 대여 가능한 자동차들의 대여비용 구하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 4️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/157339)

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

◾ CAR_RENTAL_COMPANY_CAR 테이블과 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블과 CAR_RENTAL_COMPANY_DISCOUNT_PLAN 테이블에서 자동차 종류가 '세단' 또는 'SUV'인 자동차 중 2022년 11월 1일부터 2022년 11월 30일까지 대여 가능하고 30일간의 대여 금액이 50만원 이상 200만원 미만인 자동차에 대해서 자동차 ID, 자동차 종류, 대여 금액 (컬럼명 : `FEE`) 리스트를 출력하는 SQL문을 작성해주세요. 

◾ 대여 금액을 기준으로 내림차순 정렬, 대여 금액이 같은 경우 자동차 종류를 기준으로 오름차순 정렬, 자동차 종류까지 같은 경우 자동차 ID를 기준으로 내림차순 정렬해주세요. 

## solution.sql
    SELECT * FROM (
        SELECT 
            CAR.CAR_ID, 
            CAR.CAR_TYPE,
            ROUND(((CAR.DAILY_FEE*30) * (100-PLAN.DISCOUNT_RATE) / 100), 0) AS FEE
        FROM CAR_RENTAL_COMPANY_CAR CAR
        INNER JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN PLAN ON CAR.CAR_TYPE = PLAN.CAR_TYPE
        WHERE CAR.CAR_TYPE IN ('세단', 'SUV')
        AND CAR.CAR_ID NOT IN (
            SELECT HIS.CAR_ID 
            FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY HIS
            WHERE HIS.END_DATE >= '2022-11-01'
        )
        AND PLAN.DURATION_TYPE = '30일 이상'
        ) AS TMP
    WHERE FEE BETWEEN 500000 AND 2000000
    ORDER BY FEE DESC, CAR_TYPE ASC, CAR_ID DESC

```
◾ 문제 조건 
    ▪ 차량 조건 : 세단 or SUV 
    ▪ 대여 가능 여부 : 2022년 11월에 대여 이력이 없어야 함 
    ▪ 요금 조건 : 30일간 대여 시 할인 적용한 요금이 500,000원 이상 2,000,000원 미만
    ▪ 정렬 기준 : FEE DESC, CAR_TYPE ASC, CAR_ID DESC 
◾ 컬럼별 테이블 
    ▪ 자동차 ID (CAR_ID) : CAR_RENTAL_COMPANY_CAR(CAR) / CAR_RENTAL_COMPANY_RENTAL_HISTORY(HIS)
    ▪ 자동차 종류 (CAR_TYPE) : CAR_RENTAL_COMPANY_CAR(CAR) / CAR_RENTAL_COMPANY_DISCOUNT_PLAN(PLAN)
    ▪ 대여 금액 (FEE) : CAR_RENTAL_COMPANY_DISCOUNT_PLAN(PLAN) -> 계산 과정 필요 
◾ CAR TABLE과 PLAN TABLE을 INNER JOIN 해줌으로서 출력해야 하는 조건의 COLUMNS를 만들어줍니다. 
◾ WHERE 절을 통해 '세단'과 'SUV'를 출력하였습니다.
◾ 또한 2022년 11월 1일부터 30일까지 대여가 가능해야 하기 때문에 HIS TABLE의 END_DATE가 2022-11-01보다 크게 조건을 주었습니다. 
◾ PLAN에서 30일 이상의 할인율을 주어야 하기 때문에 DURATION_TYPE = '30일 이상'으로 설정합니다.
◾ 이로서 FEE를 계산할 때 DISCOUNT_RATE가 30일 이상의 할인률로 할당됩니다. 
◾ 서브쿼리가 마친 후 FEE 조건을 WHERE로 설정하고, 정렬을 실행합니다. 
```