---

title: "[Programmers_SQL] 년, 월, 성별 별 상품 구매 회원 수 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 4️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/131532)

## TABLE 설명

◾ UESR_INFO

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`USER_ID`|INTEGER|FALSE|회원 ID|
|`GENDER`|TINYINT(1)|TRUE|성별|
|`AGE`|INTEGER|TRUE|나이|
|`JOINED`|DATE|FALSE|가입일|

▪ `GENDER` 컬름은 비어있거나 0 또는 1의 값을 가지며 0인 경우 남자를, 1인 경우는 여자를 나타냅니다. 

◾ ONLINE_SALE

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`ONLINE_SALE_ID`|INTEGER|FALSE|온라인 상품 판매 ID|
|`USER_ID`|INTEGER|FALSE|회원 ID|
|`PRODUCT_ID`|INTEGER|FALSE|상품 ID|
|`SALES_AMOUNT`|INTEGER|FALSE|판매량|
|`SALES_DATE`|DATE|FALSE|판매일|


## 문제 설명

◾ USER_INFO 테이블과 ONLINE_SALE 테이블에서 년, 월, 성별 별로 상품을 구매한 회원수를 집계하는 SQL문을 작성해주세요. 

◾ 결과는 년, 월, 성별을 기준으로 오름차순 정렬해주세요. 

◾ 성별 정보가 없는 경우는 결과에서 제외해주세요. 

## solution.sql
    SELECT DATE_FORMAT(o.sales_date, '%Y') AS year
        , DATE_FORMAT(o.sales_date, '%m') AS month
        , u.gender
        , COUNT(DISTINCT u.user_id) AS users
    FROM online_sale AS o 
        INNER JOIN user_info AS u ON o.user_id = u.user_id
    WHERE u.gender IS NOT NULL
    GROUP BY year, month, u.gender
    ORDER BY year, month, u.gender

```
◾ INNER JOIN을 통해 두 테이블을 JOIN합니다. 

◾ WHERE절을 통해 성별 정보가 없는 경우 결과에서 제외하는 조건을 수행합니다. 

◾ USERS를 구할 때, COUNT를 사용하고, GROUP BY를 통해 그룹화합니다. 

◾ ORDER BY를 통해 정해진 정렬을 수행합니다. 
```