---
title: "[Programmers_SQL] 조건에 맞는 사용자 정보 조회하기" 


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 3️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/164670)

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

◾ USED_GOODS_BOARD와 USED_GOODS_USER 테이블에서 중고 거래 게시물을 3건 이상 등록한 사용자의 사용자 ID, 닉네임, 전체주소, 전화번호를 조회하는 SQL문을 작성해주세요.

◾ 전체 주소는 시, 도로명 주소, 상세 주소가 함께 출력되도록 해주세요.

◾ 전화번호의 경우 하이픈 문자열(-)을 삽입하여 출력해주세요.

◾ 결과는 회원ID를 기준으로 내림차순 정렬해주세요. 

## solution.sql
    SELECT 
        U.USER_ID, 
        U.NICKNAME, 
        CONCAT_WS(" ", U.CITY, U.STREET_ADDRESS1, U.STREET_ADDRESS2) AS "전체주소", 
        CONCAT_WS("-", SUBSTR(U.TLNO, 1, 3), SUBSTR(U.TLNO, 4, 4), SUBSTR(U.TLNO, 8, 4)) AS "전화번호"
    FROM USED_GOODS_BOARD B
    INNER JOIN USED_GOODS_USER U
        ON B.WRITER_ID = U.USER_ID
    GROUP BY U.USER_ID
        HAVING COUNT(U.USER_ID) >= 3 
    ORDER BY U.USER_ID DESC;

```
◾ CONCAT
    ▪ 인수들을 그냥 이어붙이며, NULL이 하나라도 있으면 전체 결과가 NULL이 됩니다.
◾ CONCAT_WS
    ▪ WS는 With Separator의 약자로, 첫번째 인자는 구분자로 쓰입니다. 
    ▪ NULL값은 무시하고 연결됩니다. 
◾ SUBSTR
    ▪ 시작 위치는 1부터 시작합니다. 
    ▪ 예를 들어 SUBSTR(U.TLNO, 1, 3) 이라면 1부터 3의 길이를 가진 U.TLNO를 출력합니다. 
◾ 따라서 CONCAT_WS를 통해, 가운데 공백을 두고 전체 주소를 만들고, 하이픈 문자열을 추가하여 전화번호를 생성하였습니다.
◾ INNER JOIN을 통해 일치하는 COLUMN으로 TABLE을 JOIN 합니다. 
◾ GROUP BY 와 HAVING은 거의 묶여서 행동합니다. 
    ▪ GROUP BY를 통해 데이터를 그룹으로 만든 후 HAVING을 통해 그룹된 데이터에 조건을 취합니다. 
    ▪ GROUP BY는 SUM, COUNT와 같은 집계함수와 함께 사용하고, 다음에 HAVING을 통해 집계 조건을 필터링 합니다. 
    ▪ GROUP BY는 각 행(row)의 값을 기준으로 묶이고, HAVING은 묶인 그룹 자체에 대해 조건을 확인합니다. 
    ▪ WHERE과의 차이
        ▫ WHERE는 묶기 전에 필터링을 합니다.
        ▫ HAVING은 묶은 후 필터링을 합니다.
        ▫ 또한 WHERE에서는 집계함수를 사용할 수 없습니다. 
◾ 따라서 GROUP BY, HAVING을 통해 게시물을 3건 이상 등록한 사용자의 닉네임을 출력합니다. 
◾ 마지막으로 USER_ID로 내림차순을 해줍니다. 
```