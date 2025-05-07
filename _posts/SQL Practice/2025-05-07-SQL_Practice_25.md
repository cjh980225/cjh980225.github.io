---

title: "[Programmers_SQL] 흉부외과 또는 일반외과 의사 목록 출력하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/132203)

## TABLE 설명

◾ DOCTOR

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`DR_NAME`|VARCHAR(20)|FALSE|의사이름|
|`DR_ID`|VARCHAR(10)|FALSE|의사 ID|
|`LCNS_NO`|VARCHAR(30)|FALSE|면허번호|
|`HIRE_YMD`|DATE|FALSE|고용일자|
|`MCDP_CD`|VARCHAR(6)|TRUE|진료과코드|
|`TLNO`|VARCHAR(50)|TRUE|전화번호|

## 문제 설명

◾ DOCTOR TABLE에서 진료과가 흉부외과(CS)이거나 일반외과(GS)인 의사의 이름, 의사ID, 진료과, 고용일자를 조회하는 SQL문을 작성해주세요. 

◾ 결과는 고용일자를 기준으로 내림차순 정렬하고, 고용일자가 같다면 이름을 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT DR_NAME, DR_ID, MCDP_CD, LEFT(HIRE_YMD,10)
    FROM DOCTOR
    WHERE MCDP_CD = 'CS' OR MCDP_CD = 'GS'
    ORDER BY HIRE_YMD DESC, DR_NAME ASC;

```
◾ LEFT(HIRE_YMD, 10)
    ▪ 왼쪽부터 10글자를 출력합니다. 

◾ MCDP_CD가 CS(흉부외과) OR GD(일반외과)인 것을 필터링합니다. 

◾ 정렬의 순서에 맞춰 HIRE_YMD DESC(내림차순), DR_NAME(오름차순) 정렬합니다. 
```