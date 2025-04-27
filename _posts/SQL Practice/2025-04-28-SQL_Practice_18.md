---

title: "[Programmers_SQL] 조건에 맞는 도서와 저자 리스트 출력하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 2️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/144854)

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
|`AUTHOR_NAME`|VARCHAR(N)|FALSE|저자명명|

## 문제 설명

◾ `경제` 카테고리에 속하는 도서들의 도서 ID(`BOOK_ID`), 저자명(`AUTHOR_NAME`), 출판일(`PUBLISHED_DATE`)리스트를 출력하는 SQL문을 작성해주세요. 

◾ 출판일을 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT A.BOOK_ID, B.AUTHOR_NAME, DATE_FORMAT(A.PUBLISHED_DATE, '%Y-%m-%d') AS PUBLISHED_DATE
    FROM BOOK AS A JOIN AUTHOR AS B 
        ON A.AUTHOR_ID = B.AUTHOR_ID 
    WHERE A.CATEGORY = '경제'
    ORDER BY A.PUBLISHED_DATE;


```
◾ BOOK TABLE의 AUTHOR_ID 와 AUTHOR TABLE의 AUTHOR_ID 를 INNER JOIN 합니다. 
◾ BOOK TABLE의 경제 카테고리에 속하는 도서들을 출력하기 위해 WHERE을 사용합니다. 
◾ ORDER BY를 통해 정렬을 수행합니다. 
```