---

title: "[Programmers_SQL] 과일로 만든 아이스크림 고르기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/133025)

## TABLE 설명

◾ FIRST_HALF

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`SHIPMENT_ID`|INT(N)|FALSE|아이스크림 공장에서 아이스크림 가게까지의 출하 번호|
|`FLAVOR`|VARCHAR(N)|FALSE|아이스크림 맛|
|`TOTAL_ORDER`|INT(N)|FALSE|상반기 아이스크림 총주문량량|

◾ ICECREAM_INFO

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`FLAVOR`|VARCHAR(N)|FALSE|아이스크림 맛|
|`INGREDIENT_TYPE`|VARCHAR(N)|FALSE|아이스크림의 성분 타입입|

## 문제 설명

◾ 상반기 아이스크림 총주문량이 3,000보다 높으면서 아이스크림의 주 성분이 과일인 아이스크림의 맛을 총주문량이 큰 순서대로 조회하는 SQL문을 작성해주세요. 

## solution.sql
    SELECT A.FLAVOR
    FROM FIRST_HALF A 
        JOIN ICECREAM_INFO B
            ON A.FLAVOR = B.FLAVOR
    WHERE A.TOTAL_ORDER > 3000
        AND
        B.INGREDIENT_TYPE = "FRUIT_BASED";

```
SELECT DISTINCT INGREDIENT_TYPE
FROM ICECREAM_INFO;
```

|INGREDIENT_TYPE|
|-|
|sugar_based|
|fruit_based|

```
◾ INGREDIENT_TYPE을 확인하면 두가지 종류가 있습니다. 

◾ FLAVOR이 FIRST_HALF의 기본키, ICECREAN_INFO의 외래키이기 때문에, FLAVOR 기준으로 JOIN합니다. 

◾ WHERE절을 활용하여 TOTAL_ORDER > 3000 인 것과 INGREDIENT_TYPE = "FRUIT_BASED" 인 것을 필터링합니다. 
```