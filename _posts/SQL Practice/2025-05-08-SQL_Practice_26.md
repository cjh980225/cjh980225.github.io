---

title: "[Programmers_SQL] 진료과별 총 예약 횟수 출력하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 2️⃣  

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/132202)

## TABLE 설명

◾ APPOINTMENT

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`APNT_YMD`|TIMESTAMP|FALSE|진료예약일시|
|`APNT_NO`|NUMBER(5)|FALSE|진료예약번호|
|`PT_NO`|VARCHAR(10)|FALSE|환자번호|
|`MCDP_CD`|VARCHAR(6)|FALSE|진료과코드|
|`MDDR_ID`|VARCHAR(10)|FALSE|의사ID|
|`APNT_CNCL_YN`|VARCHAR(1)|TRUE|예약취소여부|
|`APNT_CNCL_YMD`|DATE|TRUE|예약취소날짜|

## 문제 설명

◾ APPOINTMENT 테이블에서 2022년 5월에 예약한 환자 수를 진료과코드 별로 조회하는 SQL문을 작성해주세요. 

◾ 컬럼명은 '진료과 코드', '5월예약건수'로 지정해주세요. 

◾ 결과는 진료과별 예약한 환자 수를 기준으로 오름차순 정렬하고, 예약한 환자 수가 같다면 진료과 코드를 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT MCDP_CD 진료과코드, COUNT(MCDP_CD) 5월예약건수
    FROM APPOINTMENT
    WHERE YEAR(APNT_YMD) = 2022 AND MONTH(APNT_YMD) = 5
    GROUP BY MCDP_CD
    ORDER BY 5월예약건수, MCDP_CD;

```
◾ 진료과코드 별로 → GROUP BY 
◾ 따라서 SELECT절에 COUNT(MCDP_CD)를 통해 GROUP BY의 조건을 설정합니다. 
◾ YEAR(APNT_YMD) = 2022, MONTH(APNT_YMD) = 5 로 2022년 5월 예약한 환자 수를 설정합니다. 
◾ ORDER BY 를 오름차순 순서대로 나열합니다. 
```