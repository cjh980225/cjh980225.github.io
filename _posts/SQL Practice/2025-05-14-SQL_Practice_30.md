---

title: "[Programmers_SQL] 재구매가 일어난 상품과 회원 리스트 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 2️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/131536)

## TABLE 설명

◾ ONLINE_SALE

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`ONLINE_SALE_ID`|INTEGER|FALSE|온라인 상품 판매 ID|
|`USER_ID`|INTEGER|FALSE|회원 ID|
|`PRODUCT_ID`|INTEGER|FALSE|상품 ID|
|`SALES_AMOUNT`|INTEGER|FALSE|판매량|
|`SALES_DATE`|DATE|FALSE|판매일|


## 문제 설명

◾ ONLINE_SALE 테이블에서 동일한 회원이 동일한 상품을 재구매한 데이터를 구하여, 재구매한 회원 ID와 재구매한 상품 ID를 출력하는 SQL문을 작성해주세요. 

◾ 결과는 회원 ID를 기준으로 오름차순 정렬해주시고, 회원 ID가 같다면 상품 ID를 기준으로 내림차순 정렬해주세요.

## solution.sql
    SELECT USER_ID, PRODUCT_ID
    FROM ONLINE_SALE
    GROUP BY USER_ID, PRODUCT_ID
    HAVING COUNT(PRODUCT_ID) >= 2
    ORDER BY USER_ID, PRODUCT_ID DESC;

```
◾ GROUP BY를 통해 USER_ID와 PRODUCT_ID를 그룹화합니다. 

◾ HAVING 절을 통해 GROUP BY의 조건을 설정합니다. 

    ▪ PRODUCT_ID를 했지만, USER_ID or * 을 사용하여도 결과는 동일합니다. 

    ▪ COUNT(PRODUCT_ID) 가 2개 이상인 데이터를 조건으로 설정하여 재구매한 데이터를 추출합니다. 

◾ 주어진 정렬 조건에 따라 ORDER BY를 설정합니다. 
```