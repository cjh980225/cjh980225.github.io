---

title: "[Programmers_SQL] 가격대 별 상품 개수 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 2️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/131530)

## TABLE 설명

◾ PRDDUCT

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`PRODUCT_ID`|INTEGER|FALSE|상품 ID|
|`PRODUCT_CODE`|VARCHAR(8)|FALSE|상품코드|
|`PRICE`|INTEGER|FALSE|판매가|

▪ 상품 별로 중복되지 않는 8자리 상품코드 값을 가지며, 앞 2자리는 카테고리 코드를 의미합니다. 


## 문제 설명

◾ PRODUCT 테이블에서 만원 단위의 가격대 별로 상품 개수를 출력하는 SQL문을 작성해주세요.

◾ 컬럼명은 PRICE_GROUP, PRODUCTS로 지정해주세요. 

◾ 가격대 정보는 각 구간의 최소금액으로 표시해주세요.

◾ 결과는 가격대를 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT (PRICE DIV 10000) * 10000 AS PRICE_GROUP, COUNT(*) AS PRODUCTS 
    FROM PRODUCT
    GROUP BY PRICE_GROUP
    ORDER BY PRICE_GROUP;

```
◾ PRICE DIV 10000 
    ▪ PRICE를 10000으로 나눈 값의 몫을 의미합니다. 
    ▪ 따라서 몫을 10000으로 곱하게 되면 가격대별로 그룹화가 가능해집니다. 

◾ COUNT(*)를 통해 GROUP BY로 그룹화를 진행합니다. 

◾ ORDER BY를 통해 정렬을 수행합니다. 
``` 