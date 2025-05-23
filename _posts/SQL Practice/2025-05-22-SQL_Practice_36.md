---

title: "[Programmers_SQL] 카테고리 별 상품 개수 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 2️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/131529)

## TABLE 설명

◾ PRDDUCT

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`PRODUCT_ID`|INTEGER|FALSE|상품 ID|
|`PRODUCT_CODE`|VARCHAR(8)|FALSE|상품코드|
|`PRICE`|INTEGER|FALSE|판매가|

▪ 상품 별로 중복되지 않는 8자리 상품코드 값을 가지며, 앞 2자리는 카테고리 코드를 의미합니다. 


## 문제 설명

◾ PRODUCT 테이블에서 상품 카테고리 코드(`PRODUCT_CODE` 앞 2자리) 별 상품 개수를 출력하는 SQL문을 작성해주세요.

◾ 결과는 상품 카테고리 코드를 기준으로 오름차순 정렬해주세요.

## solution.sql
    SELECT LEFT(PRODUCT_CODE, 2) AS CATEGORY, COUNT(*) AS PRODUCTS 
    FROM PRODUCT
    GROUP BY CATEGORY
    ORDER BY CATEGORY;

```
◾ LEFT(PRODUCT_CODE, 2)
    ▪ PRODUCT_CODE의 왼쪽 2자리를 출력합니다. 

◾ CATEGORY 별로 GROUP BY, COUNT(*)로 카운트하기, ORDER BY를 통해 정렬을 수행합니다. 
``` 