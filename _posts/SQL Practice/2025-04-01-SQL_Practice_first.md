---
title: "[Programmers_SQL] 조건에 부합하는 중고거래 댓글 조회하기" 


categories: SQL_Practice


tag: [python, Data_Science, Data, visualization, Preprocessing]

---

## 문제 링크

LV 1️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/164673)



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

◾ USED_GOODS_REPLY

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`REPLY_ID`|VARCHAR(10)|FALSE|댓글 ID|
|`BOARD_ID`|VARCHAR(5)|FALSE|게시글 ID|
|`WRITER_ID`|VARCHAR(50)|FALSE|작성자 ID|
|`CONTENTS`|VARCHAR(1000)|TRUE|댓글 내용|
|`CREATED_DATE`|DATE|FALSE|작성일|

## 문제 설명

◾ USED_GOODS_BOARD와 USED_GOODS_REPLY 테이블에서 2022년 10월에 작성된 게시글 제목, 게시글 ID, 댓글 ID, 댓글 작성자 ID, 댓글 내용, 댓글 작성일을 조회하는 SQL문을 작성해주세요. 

◾ 결과는 댓글 작성일을 기준으로 오름차순 정렬해주시고, 댓글 작성일이 같다면 게시글 제목을 기준으로 오름차순 정렬해주세요.

## solution.sql
    SELECT A.TITLE, 
        A.BOARD_ID, 
        B.REPLY_ID, 
        B.WRITER_ID, 
        B.CONTENTS, 
        DATE_FORMAT(B.CREATED_DATE,'%Y-%m-%d') AS CREATED_DATE
    FROM USED_GOODS_BOARD A, USED_GOODS_REPLY B
    WHERE A.BOARD_ID = B.BOARD_ID
    AND DATE_FORMAT(A.CREATED_DATE,'%Y-%m') = '2022-10'
    ORDER BY B.CREATED_DATE, A.TITLE;

```
◾ USED_GOODS_BOARD -> A   USED_GOODS_REPLY -> B 로 설정했습니다. 
◾ 두 테이블의 동일한 COLUMN인 BOARD_ID 로 테이블을 JOIN합니다. 
◾ SELECT 쿼리 실행 순서
    ▪ FROM > WHERE> GROUP BY > HAVING > SELECT > ORDER BY 
◾ 2022년 10월에 작성된 게시물을 출력하기 위해서 WHERE 을 AND로 묶어줍니다. 
    ▪ '%Y-%m'으로 날짜 형식을 일치시킵니다. 
◾ 댓글 작성일이 우선이고, 만약 같다면 게시글 제목 기준으로 오름차순 정렬입니다. 
    ▪ 따라서 ORDER BY 순서를 체크합니다. 
```