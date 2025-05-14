---

title: "[Programmers_SQL] 가장 비싼 상품 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/131697)

## TABLE 설명

◾ PRODUCT

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`PRODUCT_ID`|INTEGER|FALSE|상품 ID|
|`PRODUCT_CODE`|VARCHAR(8)|FALSE|상품코드|
|`PRICE`|INTEGER|FALSE|판매가|


## 문제 설명

◾ PRODUCT 테이블에서 판매 중인 상품 중 가장 높은 판매가를 출력하는 SQL문을 작성해주세요. 이때 컬럼명은 MAX_PRICE로 지정해주세요.  

## solution.sql
    SELECT MAX(PRICE) AS MAX_PRICE 
    FROM PRODUCT;

```
◾ PRICE의 MAX값을 출력합니다. 
```