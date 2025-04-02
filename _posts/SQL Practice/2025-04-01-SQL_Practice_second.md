---
title: "[Programmers_SQL] 조건에 부합하는 중고거래 상태 조회하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 2️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/164672)



## TABLE 설명

◾ USED_GOODS_BOARD

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`BOARD_ID`|VARCHAR(5)|FALSE|게시글 ID|
|`WRITER_ID`|VARCHAR(50)|FALSE|작성자 ID|
|`TITLE`|VARCHAR(100)|FALSE|게시글 제목|
|`CONTENTS`|VARCHAR(1000)|FALSE|게시글 내용|
|`PRICE`|NUMBER|FALSE|가격|
|`CREATED_DATE`|DATE|FALSE|작성일|
|`STATUS`|VARCHAR(10)|FALSE|거래상태|
|`VIEWS`|NUMBER|FALSE|조회수|

## 문제 설명

◾ USED_GOODS_BOARD 테이블에서 2022년 10월 5일에 등록된 중고거래 게시물의 게시글 ID, 작성자 ID, 게시글 제목, 가격, 거래상태를 조회하는 SQL문을 작성해주세요. 

◾ 거래상태가 SALE 이면 판매중, RESERVED이면 예약중, DONE이면 거래완료 분류하여 출력해주세요. 

◾ 결과는 게시글 ID 기준으로 내림차순 정렬해주세요. 

## solution.sql
    SELECT BOARD_ID, WRITER_ID, TITLE, PRICE,
        (CASE
            WHEN STATUS = "DONE" THEN '거래완료'
            WHEN STATUS = "SALE" THEN '판매중'
            WHEN STATUS = "RESERVED" THEN '예약중' END) AS STATUS
    FROM USED_GOODS_BOARD
    WHERE DATE_FORMAT(CREATED_DATE, '%Y-%m-%d') = "2022-10-05"
    ORDER BY BOARD_ID DESC 
    ;

```
◾ CASE문을 사용하는 경우 
    ▪ 약어나 코드를 읽기 쉬운 값으로 바꿔줄 때 사용합니다. 
    ▪ 데이터를 범주화할 때 사용합니다. 
    ▪ 현재는 읽기 쉬운 값으로 바꾸기 위해 사용되었습니다. 
◾ WHERE 문을 통해 2022년 10월 5일로 설정했습니다.     
◾ ORDER BY 의 DEFAULT 값은 오름차순이며, 내림차순의 조건을 만족하기 위하여 DESC를 추가했습니다.     
```