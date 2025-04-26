---
title: "[Programmers_SQL] 자동차 종류 별 특정 옵션이 포함된 자동차 수 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 2️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/151137)

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

## 문제 설명

◾ CAR_RENTAL_COMPANY_CAR 테이블에서 '통풍시트', '열선시트', '가죽시트' 중 하나 이상의 옵션이 포함된 자동차가 자동차 종류 별로 몇대인지 출력하는 SQL문을 작성해주세요. 

◾ 자동차 수에 대한 컬럼명은 `CARS`로 지정해주세요. 

◾ 결과는 자동차 종류를 기준으로 오름차순으로 정렬해주세요. 

## solution.sql
    SELECT CAR_TYPE, COUNT(*) AS CARS
    FROM CAR_RENTAL_COMPANY_CAR
    WHERE OPTIONS LIKE '%통풍시트%' OR OPTIONS LIKE '%열선시트%' OR OPTIONS LIKE '%가죽시트%'
    GROUP BY CAR_TYPE
    ORDER BY CAR_TYPE;


```
◾ GROUP BY 로 CAR_TYPE을 그룹화 합니다. 
◾ LIKE '%통풍시트%' 
    ▪ %가 양 끝으로 들어가는 것은 앞, 뒤로 어떠한 내용이 있더라도 통풍시트가 있으면 선택하는 옵션입니다. 
```
