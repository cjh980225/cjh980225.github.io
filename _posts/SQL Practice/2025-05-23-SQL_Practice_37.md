---

title: "[Programmers_SQL] 나이 정보가 없는 회원 수 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/131528)

## TABLE 설명

◾ USER_INFO

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`USER_ID`|INTEGER|FALSE|회원 ID|
|`GENDER`|TINYINT(1)|TRUE|성별|
|`AGE`|INTEGER|TRUE|나이|
|`JOINED`|DATE|FALSE|가입일|

▪ GENDER 컬럼은 비어있거나 0 또는 1의 값을 가지며 0인 경우 남자를, 1인 경우는 여자를 나타냅니다. 


## 문제 설명

◾ USER_INFO 테이블에서 나이 정보가 없는 회원이 몇 명인지 출력하는 SQL문을 작성해주세요. 

◾ 컬럼명은 USERS로 지정해주세요. 

## solution.sql
    SELECT COUNT(USER_ID) AS USERS
    FROM USER_INFO
    WHERE AGE IS NULL

```
◾ WHERE AGE IS NULL
    ▪ AGE 가 NULL인 조건입니다. 
``` 