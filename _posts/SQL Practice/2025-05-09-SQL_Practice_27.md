---

title: "[Programmers_SQL] 12세 이하인 여자 환자 목록 출력하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/132201)

## TABLE 설명

◾ PATIENT

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`PT_NO`|VARCHAR(10)|FALSE|환자번호|
|`PT_NAME`|VARCHAR(20)|FALSE|환자이름|
|`GEND_CD`|VARCHAR(1)|FALSE|성별코드|
|`AGE`|INTEGER|FALSE|나이|
|`TLNO`|VARCHAR(50)|TRUE|전화번호|

## 문제 설명

◾ PATIENT 테이블에서 12세 이하인 여자환자의 환자이름, 환자번호, 성별코드, 나이, 전화번호를 조회하는 SQL문을 작성해주세요. 

◾ 이때 전화번호가 없는 경우, 'NONE'으로 출력시켜 주세요. 

◾ 결과는 나이를 기준으로 내림차순 정렬하고, 나이가 같다면 환자이름을 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT PT_NAME, PT_NO, GEND_CD, AGE, COALESCE(TLNO, 'NONE') AS TLNO
    FROM PATIENT
    WHERE AGE <= 12 AND GEND_CD = 'W'
    ORDER BY AGE DESC, PT_NAME ASC;

```
◾ COALESCE(TLNO, 'NONE') AS TLNO
    ▪ TLNO, 만약 없으면 NONE 값을 출력합니다. 
◾ WHERE 절을 통해 12세 이하인 여자환자를 필터링합니다. 
◾ ORDER BY를 통해 나이를 기준으로 내림차순 정렬, 환자이름을 기준으로 오름차순 정렬을 합니다. 
```