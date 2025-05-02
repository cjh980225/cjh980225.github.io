---

title: "[Programmers_SQL] 주문량이 많은 아이스크림들 조회하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 4️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/133027)

## TABLE 설명

◾ FIRST_HALF

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`SHIPMENT_ID`|INT(N)|FALSE|아이스크림 공장에서 아이스크림 가게까지의 출하 번호|
|`FLAVOR`|VARCHAR(N)|FALSE|아이스크림 맛|
|`TOTAL_ORDER`|INT(N)|FALSE|상반기 아이스크림 총주문량|

◾ JULY

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`SHIPMENT_ID`|INT(N)|FALSE|아이스크림 공장에서 아이스크림 가게까지의 출하 번호|
|`FLAVOR`|VARCHAR(N)|FALSE|아이스크림 맛|
|`TOTAL_ORDER`|INT(N)|FALSE|7월 아이스크림 총주문량|

## 문제 설명

◾ 7월 아이스크림 총 주문량과 상반기의 아이스크림 총 주문량을 더한 값이 큰 순서대로 상위 3개의 맛을 조회하는 SQL문을 작성해주세요. 

## solution.sql
    SELECT FLAVOR
    FROM (SELECT * 
        FROM FIRST_HALF 
            UNION ALL
        SELECT *
        FROM JULY) A
    GROUP BY FLAVOR
    ORDER BY SUM(TOTAL_ORDER) DESC
    LIMIT 3;


```
◾ UNION ALL 을 통해 중복되는 내용도 포함하여 두 테이블을 UNION 합니다. 

◾ FLAVOR을 통해 GROUP BY를 합니다. 

◾ ORDER BY에서 TOTAL_ORDER을 통해 그룹화한 FLAVOR의 총 금액을 SUM 한 이후, 내림차순 정렬합니다. 

◾ LIMIT 3으로 상위 세줄을 출력합니다. 
```