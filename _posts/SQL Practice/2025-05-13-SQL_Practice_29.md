---

title: "[Programmers_SQL] 오프라인/온라인 판매 데이터 통합하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 4️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/131537)

## TABLE 설명

◾ ONLINE_SALE

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`ONLINE_SALE_ID`|INTEGER|FALSE|온라인 상품 판매 ID|
|`USER_ID`|INTEGER|FALSE|회원 ID|
|`PRODUCT_ID`|INTEGER|FALSE|상품 ID|
|`SALES_AMOUNT`|INTEGER|FALSE|판매량|
|`SALES_DATE`|DATE|FALSE|판매일|

▪ 동일한 날짜, 회원ID, 상품ID 조합에 대해서는 하나의 판매 데이터만 존재합니다. 

◾ OFFLINE_SALE

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`OFFLINE_SALE_ID`|INTEGER|FALSE|오프라인 상품 판매 ID|
|`PRODUCT_ID`|INTEGER|FALSE|상품 ID|
|`SALES_AMOUNT`|INTEGER|FALSE|판매량|
|`SALES_DATE`|DATE|FALSE|판매일|

## 문제 설명

◾ ONLINE_SALE 테이블과 OFFLINE_SALE 테이블에서 2022년 3월의 오프라인/온라인 상품 판매 데이터의 판매 날짜, 상품 ID, 유저 ID, 판매량을 출력하는 SQL문을 작성해주세요. 

◾ OFFLINE_SALE 테이블의 판매 데이터의 USER_ID 값은 NULL로 표시해주세요. 

◾ 결과는 판매일을 기준으로 오름차순 정렬해주시고, 판매일이 같다면 상품 ID를 기준으로 오름차순, 상품 ID까지 같다면 유저 ID를 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT *
    FROM 
    (SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE, 
        PRODUCT_ID, 
        USER_ID, 
        SALES_AMOUNT
    FROM ONLINE_SALE

    UNION ALL

    SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE, 
        PRODUCT_ID, 
        NULL AS USER_ID, 
        SALES_AMOUNT
    FROM OFFLINE_SALE) A
    WHERE SALES_DATE LIKE '2022-03%'
    ORDER BY SALES_DATE, PRODUCT_ID, USER_ID;

```
◾ ONLINE_SALE 테이블과 OFFLINE_SALE 테이블의 차이점은 USER_ID 존재 유무입니다. 따라서 OFFLINE_SALE 테이블의 USER_ID를 NULL로 설정합니다. 

◾ 두 테이블을 UNION ALL을 통해 중복이 되어도 한 테이블에 쌓일 수 있게 설정합니다. 

◾ NULL AS USER_ID 를 통해 USER_ID 를 NULL로 설정합니다. 

◾ 2022년 03월의 데이터를 출력하기 위하여 WHERE 절을 통해 설정합니다. 

◾ ORDER BY를 통해 조건에 맞는 정렬을 수행합니다. 
```