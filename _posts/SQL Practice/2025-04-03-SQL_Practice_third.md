---
title: "[Programmers_SQL] 조회수가 가장 많은 중고거래 게시판의 첨부파일 조회하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 3️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/164671)



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

◾ USED_GOODS_FILE

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`FILE_ID`|VARCHAR(10)|FALSE|파일 ID|
|`FILE_EXT`|VARCHAR(5)|FALSE|파일 확장자|
|`FILE_NAME`|VARCHAR(256)|FALSE|파일 이름|
|`BOARD_ID`|VARCHAR(10)|FALSE|게시글 ID|

## 문제 설명

◾ USED_GOODS_BOARD와 USED_GOODS_FILE 테이블에서 조회수가 가장 높은 중고거래 게시물에 대한 첨부파일 경로를 조회하는 SQL문을 작성해주세요. 

◾첨부파일 경로는 FILE ID를 기준으로 내림차순 정렬해주세요.

◾ 기본적인 파일경로는 /home/grep/src/ 이며, 게시글 ID 기준으로 디렉토리가 구분되고, 파일이름은 파일 ID, 파일 이름, 파일 확장자로 구성되도록 출력해주세요. 

◾ 조회수가 가장 높은 게시물은 하나만 존재합니다. 

## solution.sql
    SELECT CONCAT('/home/grep/src/', BOARD_ID, '/', FILE_ID, FILE_NAME, FILE_EXT) AS FILE_PATH
    FROM USED_GOODS_FILE
    WHERE BOARD_ID =
        (
        SELECT BOARD_ID
        FROM USED_GOODS_BOARD
        WHERE VIEWS = 
            (
            SELECT MAX(VIEWS) FROM USED_GOODS_BOARD
            )
        )
    ORDER BY FILE_ID DESC;

```
◾ CONCAT : 문자형으로 변환 후 합치는 작업이 이뤄집니다. 
◾ 속 쿼리부터 보면 USED_GOODS_BOARD에서 최고 뷰를 가져옵니다. 
◾ 그 다음 쿼리는 BOARD_ID 를 출력합니다. 
◾ 따라서 USED_GOODS_FILE에서 BOARD_ID 는 USED_GOODS_BOARD에서 최고뷰를 가진 것이 됩니다. 
◾ FILE_ID 로 내림차순을 해줍니다. 
```