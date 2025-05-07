---

title: "[Programmers_SQL] 성분으로 구분한 아이스크림 총 주문량"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 2️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/133026)

## TABLE 설명

◾ FIRST_HALF

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`SHIPMENT_ID`|INT(N)|FALSE|아이스크림 공장에서 아이스크림 가게까지의 출하 번호|
|`FLAVOR`|VARCHAR(N)|FALSE|아이스크림 맛|
|`TOTAL_ORDER`|INT(N)|FALSE|상반기 아이스크림 총 주문량|

◾ ICECREAM_INFO

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`FLAVOR`|VARCHAR(N)|FALSE|아이스크림 맛|
|`INGREDIENT_TYPE`|VARCHAR(N)|FALSE|아이스크림의 성분 타입|

## 문제 설명

◾ 상반기 동안 각 아이스크림 성분 타입과 성분 타입에 대한 아이스크림의 총주문량을 총주문량이 작은 순서대로 조회하는 SQL문을 작성해주세요. 

◾ 총주문량을 나타내는 컬럼명은 TOTAL_ORDER로 지정해주세요. 

## solution.sql
    SELECT B.INGREDIENT_TYPE, SUM(A.TOTAL_ORDER) AS TOTAL_ORDER 
    FROM FIRST_HALF AS A JOIN ICECREAM_INFO AS B
        ON A.FLAVOR = B.FLAVOR 
    GROUP BY B.INGREDIENT_TYPE
    ORDER BY TOTAL_ORDER;


```
◾ ICECREAM_INFO TABLE의 FLAVOR은 FIRST_HALF TABLE의 FLAVOR의 외래키입니다. 

◾ 따라서 FLAVOR을 기준으로 두 테이블을 JOIN합니다. 

◾ GROUP BY로 아이스크림 성분 타입을 그룹화해주고, 총 주문량을 출력하기 위해 
    SELECT 부분에 SUM(A.TOTAL_ORDER)을 통해 총 주문량을 계산합니다. 
    ▪ SUM(A.TOTAL_ORDER)을 TOTAL_ORDER로 COLUMN명을 설정합니다. 

◾ TOTAL_ORDER을 오름차순으로 정렬하며 총 주문량이 작은 순서대로 출력합니다. 
```