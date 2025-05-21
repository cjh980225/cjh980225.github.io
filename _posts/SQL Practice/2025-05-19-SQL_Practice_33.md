---

title: "[Programmers_SQL] 상품별 오프라인 매출 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 2️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/131533)

## TABLE 설명

◾ PRDDUCT

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`PRODUCT_ID`|INTEGER|FALSE|상품 ID|
|`PRODUCT_CODE`|VARCHAR(8)|FALSE|상품코드|
|`PRICE`|INTEGER|FALSE|판매가|

▪ 상품 별로 중복되지 않는 8자리 상품코드 값을 가지며, 앞 2자리는 카테고리 코드를 의미합니다. 

◾ OFFLINE_SALE

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`OFFLINE_SALE_ID`|INTEGER|FALSE|오프라인 상품 판매 ID|
|`PRODUCT_ID`|INTEGER|FALSE|상품 ID|
|`SALES_AMOUNT`|INTEGER|FALSE|판매량|
|`SALES_DATE`|DATE|FALSE|판매일|

▪ 동일한 날짜, 상품 ID 조합에 대해서는 하나의 판매 데이터만 존재합니다. 

## 문제 설명

◾ PRODUCT 테이블과 OFFLINE_SALE 테이블에서 상품코드 별 매출액(판매가 * 판매량) 합계를 출력하는 SQL문을 작성해주세요. 

◾ 결과는 매출액을 기준으로 내림차순 정렬해주시고 매출액이 같다면 상품코드를 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT A.PRODUCT_CODE, SUM(A.PRICE * B.SALES_AMOUNT) AS SALES
    FROM PRODUCT A
        JOIN OFFLINE_SALE B
            ON A.PRODUCT_ID = B.PRODUCT_ID 
    GROUP BY A.PRODUCT_CODE
    ORDER BY SALES DESC, A.PRODUCT_CODE ASC;

```
◾ PRODUCT 테이블과 OFFLINE_SALE 테이블을 PRODUCT_ID 컬럼으로 JOIN합니다. 

◾ SUM(판매가 * 판매량)을 기준으로 PRODUCT_CODE을 그룹화합니다. 

◾ ORDER BY를 통해 주어진 정렬을 수행합니다. 
```