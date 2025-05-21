---

title: "[Programmers_SQL] 상품을 구매한 회원 비율 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 5️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/131534)

## TABLE 설명

◾ UESR_INFO

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`USER_ID`|INTEGER|FALSE|회원 ID|
|`GENDER`|TINYINT(1)|TRUE|성별|
|`AGE`|INTEGER|TRUE|나이|
|`JOINED`|DATE|FALSE|가입일|

▪ `GENDER` 컬럼은 비어있거나 0 또는 1의 값을 가지며 0인 경우 남자를, 1인 경우는 여자를 나타냅니다. 

◾ ONLINE_SALE

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`ONLINE_SALE_ID`|INTEGER|FALSE|온라인 상품 판매 ID|
|`USER_ID`|INTEGER|FALSE|회원 ID|
|`PRODUCT_ID`|INTEGER|FALSE|상품 ID|
|`SALES_AMOUNT`|INTEGER|FALSE|판매량|
|`SALES_DATE`|DATE|FALSE|판매일|


## 문제 설명

◾ USER_INFO 테이블과 ONLINE_SALE 테이블에서 2021년에 가입한 전체 회원들 중 상품을 구매한 회원수와 상품을 구매한 회원의 비율 (= 2021년에 가입한 회원 중 상품을 구매한 회원수 / 2021년에 가입한 전체 회원 수)을 년, 월 별로 출력하는 SQL문을 작성해주세요. 

◾ 상품을 구매한 회원의 비율은 소수점 두번째 자리에서 반올림하고, 전체 결과는 년을 기준으로 오름차순 정렬해주시고 년이 같다면 월을 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT SUBSTRING(OS.SALES_DATE,1,4) YEAR
            ,SUBSTRING(OS.SALES_DATE,6,2) MONTH
            ,COUNT(distinct OS.USER_ID) PURCHASED_USRES
            ,ROUND(COUNT(distinct OS.USER_ID) / (SELECT COUNT(*) FROM USER_INFO WHERE JOINED BETWEEN '2021-01-01' and '2022-01-01' ),1) PUCHASED_RATIO
    FROM USER_INFO UR
        LEFT JOIN ONLINE_SALE OS ON UR.USER_ID = OS.USER_ID
    WHERE UR.JOINED BETWEEN '2021-01-01' and '2022-01-01'
    AND OS.SALES_DATE IS NOT NULL
    GROUP BY SUBSTRING(OS.SALES_DATE,1,4)
            ,SUBSTRING(OS.SALES_DATE,6,2)
    ORDER BY YEAR ASC, MONTH ASC

```
◾ SELECT 절 
    ▪ SUBSTRING(OS.SALES_DATE, 1, 4) YEAR 
        ▫ SALES_DATE 문자열에서 앞 4자리(연도)를 추출
    ▪ SUBSTRING(OS.SALES_DATE, 6, 2) MONTH 
        ▫ SALES_DATE 문자열에서 6번째 문자부터 2자리(월)를 추출
    ▪ COUNT(DISTINCT OS.USER_ID) PURCHASED_USERS
        ▫ 해당 월에 구매한 사용자 수(중복 제거)를 COUNT
        ▫ 한 사용자가 여러 번 사더라도 한 번만 계산
    ▪ ROUND(..., 1) PURCHASED_USERS
        ▫ 구매한 사용자 수 / 2021년 가입자 수 전체 비율 계산 후, 소수점 첫째 자리까지 반올림
        ▫ BETWEEN을 통해 2021-01-01 ~ 2021-12-31 사이로 필터링

◾ FROM 및 JOIN 절
    ▪ USER_INFO 테이블에서 사용자 정보를 가져오고, ONLINE_SALE 테이블과 USER_ID를 기준으로 LEFT_JOIN
    ▪ 즉, 가입한 유저 중에서 구매 정보가 있는 사람만 확인할 수 있도록 함 

◾ WHERE 절 
    ▪ JOINED BETWEEN '2021-01-01' AND '2022-01-01'
        ▫ 2021년에 가입한 사용자들만 필터링
        ▫ 또한, 구매 정보가 실제로 존재하는 경우만 고려 

◾ GROUP BY 절 
    ▪ 연도, 월 단위로 그룹화하여 집계 

◾ ORDER BY 절 
    ▪ 연도 → 월 순으로 오름차순 정렬
```