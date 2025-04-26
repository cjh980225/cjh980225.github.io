---

title: "[Programmers_SQL] 저자 별 카테고리 별 매출액 집계하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 4️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/144856)

## TABLE 설명

◾ BOOK

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`BOOK_ID`|INTEGER|FALSE|도서 ID|
|`CATEGORY`|VARCHAR(N)|FALSE|카테고리 (경제, 인문, 소설, 생활, 기술)|
|`AUTHOR_ID`|INTEGER|FALSE|저자 ID|
|`PRICE`|INTEGER|FALSE|판매가 (원)|
|`PUBLISHED_DATE`|DATE|FALSE|출판일|

◾ AUTHOR

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`AUTHOR_ID`|INTEGER|FALSE|저자 ID|
|`AUTHOR_NAME`|VARCHAR(N)|FALSE|저자명|

◾ BOOK_SALES

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`BOOK_ID`|INTEGER|FALSE|도서 ID|
|`SALES_DATE`|DATE|FALSE|판매일|
|`SALES`|INTEGER|FALSE|판매량|

## 문제 설명

◾ `2022년 1월`의 도서 판매 데이터를 기준으로 저자 별, 카테고리 별 매출액(`TOTAL_SALES = 판매량 * 판매가`)을 구하여,저자 ID(`AUTHOR_ID`), 저자명(`AUTHOR_NAME`), 카테고리(`CATEGORY`), 매출액(`SALES`) 리스트를 출력하는 SQL문을 작성해주세요. 

◾ 저자 ID를 오름차순으로, 저자 ID가 같다면 카테고리를 내림차순 정렬해주세요. 

## solution.sql
    SELECT B.AUTHOR_ID, B.AUTHOR_NAME, A.CATEGORY, SUM(A.PRICE * C.SALES) AS TOTAL_SALES 
    FROM BOOK A
        JOIN AUTHOR B
            ON A.AUTHOR_ID = B.AUTHOR_ID
        JOIN BOOK_SALES C
            ON A.BOOK_ID = C.BOOK_ID
    WHERE YEAR(C.SALES_DATE) = 2022
        AND
            MONTH(C.SALES_DATE) = 1
    GROUP BY B.AUTHOR_NAME, A.CATEGORY
    ORDER BY B.AUTHOR_ID, A.CATEGORY DESC ;


```
◾ JOIN은 INNER JOIN과 같은 역할을 합니다. 
◾ BOOK, AUTHOR, BOOK_SALES 세 개의 테이블을 JOIN 한 후, WHERE 절로 조건을 걸어줍니다. 
◾ DATE TYPE에 YEAR, MONTH을 출력하며 2022년 1월로 범위를 설정합니다. 
◾ 저자 별, 카테고리 별로 출력하기 위해 GROUP BY를 통해 그룹화를 진행합니다. 
◾ 최종적으로 ORDER BY 를 통해 정렬을 합니다. 
```