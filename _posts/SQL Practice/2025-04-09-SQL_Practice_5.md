---
title: "[Programmers_SQL] 조건에 맞는 사용자와 총 거래금액 조회하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 3️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/164668)

## TABLE 설명

◾ USED_GOODS_BOARD

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`BOARD_ID`|VARCHAR(5)|FALSE|게시글ID|
|`WRITER_ID`|VARCHAR(50)|FALSE|작성자ID|
|`TITLE`|VARCHAR(100)|FALSE|게시글 제목|
|`CONTENTS`|VARCHAR(1000)|FALSE|게시글 내용|
|`PRICE`|NUMBER|FALSE|가격|
|`CREATED_DATE`|DATE|FALSE|작성일|
|`STATUS`|VARCHAR(10)|FALSE|거래상태|
|`VIEWS`|NUMBER|FALSE|조회수|

◾ USED_GOODS_USER

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`USED_ID`|VARCHAR(50)|FALSE|회윈ID|
|`NICKNAME`|VARCHAR(100)|FALSE|닉네임|
|`CITY`|VARCHAR(100)|FALSE|시|
|`STREET_ADDRESS1`|VARCHAR(100)|FALSE|도로명 주소|
|`STREET_ADDRESS2`|VARCHAR(100)|TRUE|상세 주소|
|`TLNO`|VARCHAR(20)|FALSE|전화번호|

## 문제 설명

◾ USED_GOODS_BOARD와 USED_GOODS_USER 테이블에서 완료된 중고 거래의 총 금액이 70만원 이상인 사람의 회원 ID, 닉네임, 총거래금액을 조회하는 SQL문을 작성해주세요. 

◾ 총거래금액을 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT B.USER_ID, B.NICKNAME, SUM(A.PRICE) AS TOTAL_SALES
    FROM USED_GOODS_BOARD A 
        JOIN USED_GOODS_USER B
            ON A.WRITER_ID = B.USER_ID
    WHERE A.STATUS = 'DONE'
    GROUP BY B.USER_ID, B.NICKNAME
        HAVING TOTAL_SALES >= 700000
    ORDER BY TOTAL_SALES ;

```
◾ SUM(PRICE)
    ▪ 집계함수가 있기 때문에 GROUP BY에서 어떤 상태일 때 SUM을 해주는 것인지 표현해야 합니다. 
    ▪ 따라서 GROUP BY에서 USER_ID와 NICKNAME일 때 SUM(PRICE)를 해주는 것입니다. 
◾ A.WRITER_ID = B.USER_ID
    ▪ 같은 컬럼을 통해 두 TABLE을 JOIN 합니다. 
◾ WHERE STATUS = 'DONE'
    ▪ WHERE은 묶기 전에 필터링을 합니다. 
◾ HAVING TOTAL_SALES >= 700000
    ▪ HAVING은 묶은 후 필터링을 합니다. 
◾ 마지막으로 TOTAL_SALES로 오름차순을 해줍니다.        
```