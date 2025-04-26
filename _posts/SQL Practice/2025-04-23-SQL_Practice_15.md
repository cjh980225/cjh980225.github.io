---

title: "[Programmers_SQL] 평균 일일 대여 요금 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/151136)

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

◾ CAR_RENTAL_COMPANY_CAR 테이블에서 자동차 종류가 'SUV'인 자동차들의 평균 일일 대여 요금을 출력하는 SQL문을 작성해주세요. 

◾ 평균 일일 대여 요금은 소수 첫 번째 자리에서 반올림하고, 컬럼명은 `AVERAGE_FEE`로 지정해주세요.  

## solution.sql
    SELECT ROUND(AVG(DAILY_FEE)) AS AVERAGE_FEE
    FROM CAR_RENTAL_COMPANY_CAR
    WHERE CAR_TYPE = "SUV";


```
◾ AVG 
	▪ DAILY_FEE의 평균을 구합니다. 
◾ROUND 
	▪ 평균값이 소수점 자리수를 설정합니다. 
	▪ Default 값이 0이기 때문에 소수 첫 번재 자리에서 반올림하는 조건을 만족합니다. 
◾WHERE 로 CAR_TYPE를 SUV로 설정하게 되면 SUV인 자동차들의 평균 일일 대여 요금을 출력하게 됩니다.
```