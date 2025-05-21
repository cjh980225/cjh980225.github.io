---

title: "[Programmers_SQL] 조건에 맞는 회원수 구하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/131535)

## TABLE 설명

◾ USER_INFO

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`USER_ID`|INTEGER|FALSE|회원 ID|
|`GENDER`|TINYINT(1)|TRUE|성별|
|`AGE`|INTEGER|TRUE|나이|
|`JOINED`|DATE|FALSE|가입일|

▪ `GENDER` 컬름은 비어있거나 0 또는 1의 값을 가지며 0인 경우 남자를, 1인 경우는 여자를 나타냅니다. 

## 문제 설명

◾ USER_INFO 테이블에서 2021년에 가입한 회원 중 나이가 20세 이상 29세 이하인 회원이 몇 명인지 출력하는 SQL문을 작성해주세요. 

## solution.sql
    SELECT COUNT(USER_ID) AS USERS
    FROM USER_INFO 
    WHERE AGE >= 20 AND AGE <= 29
        AND 
        LEFT(JOINED, 4) = 2021;

```
◾ USER_ID가 몇 명인지 출력하는 문제이기 때문에 COUNT(USER_ID)를 USERS로 설정합니다. 

◾ USER_INFO 테이블에서 AGE가 20세 이상, 29세 이하인 사람을 WHERE을 통해 필터링합니다. 

◾ LEFT(JOINED, 4) 를 통해 가입일 데이터의 왼쪽부터 4글짜 (년도)가 2021년인 경우를 COUNT합니다. 
```