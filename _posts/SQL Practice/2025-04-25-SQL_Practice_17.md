---

title: "[Programmers_SQL] 카테고리 별 도서 판매량 집계하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 3️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/144855)

## TABLE 설명

◾ BOOK

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`BOOK_ID`|INTEGER|FALSE|도서 ID|
|`CATEGORY`|VARCHAR(N)|FALSE|카테고리 (경제, 인문, 소설, 생활, 기술)|
|`AUTHOR_ID`|INTEGER|FALSE|저자 ID|
|`PRICE`|INTEGER|FALSE|판매가 (원)|
|`PUBLISHED_DATE`|DATE|FALSE|출판일|

◾ BOOK_SALES

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`BOOK_ID`|INTEGER|FALSE|도서 ID|
|`SALES_DATE`|DATE|FALSE|판매일|
|`SALES`|INTEGER|FALSE|판매량|

## 문제 설명

◾ `2022년 1월`의 카테고리 별 도서 판매량을 합산하고, 카테고리(CATEGORY), 총 판매량(TOTAL_SALES) 리스트를 출력하는 SQL문을 작성해주세요. 

◾ 카테고리명을 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT A.CATEGORY, SUM(B.SALES) AS TOTAL_SALES
    FROM BOOK A
        JOIN BOOK_SALES B
            ON A.BOOK_ID = B.BOOK_ID
    WHERE B.SALES_DATE LIKE '%2022-01%'
    GROUP BY A.CATEGORY
    ORDER BY A.CATEGORY;


```
◾ BOOK TABLE과 BOOK_SALES TABLE을 INNER JOIN 합니다. 
    ▪ 같은 COLUMN인 BOOK_ID로 JOIN 합니다. 
◾ 이전 문제에서는 YEAR(SALES_DATE), MONTH(SALES_DATE)로 WHERE 절을 설정했었습니다. 
◾ 이번에는 LIKE, % 를 통해 2022년 01월이 들어있는 곳을 찾습니다. 
◾ 카테고리 별 도서판매량을 합산하는 것이기 때문에 CATEGORY로 그룹화를 설정합니다.
◾ ORDER BY를 통해 CATEGORY 기준으로 오름차순 정렬합니다.   
```