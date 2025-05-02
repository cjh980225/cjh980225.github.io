---

title: "[Programmers_SQL] 조건에 맞는 도서 리스트 출력하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/144853)

## TABLE 설명

◾ BOOK

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`BOOK_ID`|INTEGER|FALSE|도서 ID|
|`CATEGORY`|VARCHAR(N)|FALSE|카테고리 (경제, 인문, 소설, 생활, 기술)|
|`AUTHOR_ID`|INTEGER|FALSE|저자 ID|
|`PRICE`|INTEGER|FALSE|판매가 (원)|
|`PUBLISHED_DATE`|DATE|FALSE|출판일|

## 문제 설명

◾ `Book` Table에서 `2021년`에 출판된 `인문` 카테고리에 속하는 도서 리스트를 찾아서 도서 ID(`BOOK_ID`), 출판일(`PUBLISHED_DATE`)을 출력하는 SQL문을 작성해주세요. 

◾ 결과는 출판일을 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT
        BOOK_ID
        ,DATE_FORMAT(PUBLISHED_DATE,"%Y-%m-%d") AS PUBLISHED_DATE
    FROM BOOK 
    WHERE 
        PUBLISHED_DATE >= '2021-01-01 00:00:00' AND PUBLISHED_DATE < '2022-01-01 00:00:00'
        AND category = '인문'
    ORDER BY 2


```
◾ 도서 ID, 출판일을 SELECT 합니다. 
    ▪ DATE_FORMAT을 통해 날짜 출력 형식을 조건에 따라 설정합니다. 

◾ 2021년에 출판된 인문 카테고리를 출력해야하므로, WHERE절을 통해 설정합니다. 

◾ ORDER BY PUBLISHED_DATE를 통해 출판일을 기준으로 오름차순합니다. 
    ▪ ORDER BY 2 에서 2의 의미는 SELECT의 두번째 COLUMN을 의미합니다. 
```