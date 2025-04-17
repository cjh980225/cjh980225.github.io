---
title: "[Programmers_SQL] 대여 기록이 존재하는 자동차 리스트 구하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 3️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/157341)

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

## 문제 설명

◾ CAR_RENTAL_COMPANY_CAR 테이블과 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 자동차 종류가 '세단'인 자동차들 중 10월에 대여를 시작한 기록이 있는 자동차 ID 리스트를 출력하는 SQL문을 작성해주세요. 

◾ 자동차 ID 리스트는 중복이 없어야 합니다.

◾ 자동차 ID를 기준으로 내림차순 정렬해주세요. 

## solution.sql
    SELECT DISTINCT B.CAR_ID 
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY A
        JOIN CAR_RENTAL_COMPANY_CAR B
            ON A.CAR_ID = B.CAR_ID
    WHERE MONTH(START_DATE) = 10
        AND
            B.CAR_TYPE = '세단'
    ORDER BY CAR_ID DESC;

```
◾ DISTINCT 
    ▪ 특정 컬럼의 유니크한 값을 조회하는 문법입니다. 
    ▪ 자동차 ID 리스트는 중복이 없어야 한다는 조건이 있기 때문에 DISTINCT를 사용합니다. 
◾ CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블과 CAR_RENTAL_COMPANY_CAR의 동일 컬럼인 CAR_ID로 JOIN합니다. 
◾ MONTH(START_DATE) = 10
    ▪ START_DATE의 월만 출력합니다. 
◾ B.CAR_TYPE = '세단'
    ▪ CAR_TYPE이 세단인 것만 출력합니다.
◾ CAR_ID로 내림차순 정렬합니다. 
```