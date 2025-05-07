---

title: "[Programmers_SQL] 취소되지 않은 진료 예약 조회하기"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 4️⃣

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/132204)

## TABLE 설명

◾ PATIENT

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`PT_NO`|VARCHAR(N)|FALSE|환자번호|
|`PT_NAME`|VARCHAR(N)|FALSE|환자이름|
|`GEND_CD`|VARCHAR(N)|FALSE|성별코드|
|`AGE`|INTEGER|FALSE|나이|
|`TLNO`|VARCHAR(N)|TRUE|전화번호|

◾ DOCTOR

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`DR_NAME`|VARCHAR(N)|FALSE|의사이름|
|`DR_ID`|VARCHAR(N)|FALSE|의사ID|
|`LCNS_NO`|VARCHAR(N)|FALSE|면허번호|
|`HIRE_YMD`|DATE|FALSE|고용일자|
|`MCDP_CD`|VARCHAR(N)|TRUE|진료과코드|
|`TLNO`|VARCHAR(N)|TRUE|전화번호|

◾ APPOINTMENT

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`APNT_YMD`|TIMESTAMP|FALSE|진료 예약일시|
|`APNT_NO`|INTEGER|FALSE|진료예약번호|
|`PT_NO`|VARCHAR(N)|FALSE|환자번호|
|`MCDP_CD`|VARCHAR(N)|FALSE|진료과코드|
|`MDDR_ID`|VARCHAR(N)|FALSE|의사ID|
|`APNT_CNCL_YN`|VARCHAR(N)|TRUE|예약취소여부|
|`APNT_CNCL_YMD`|DATE|TRUE|예약취소날짜짜|

## 문제 설명

◾ PATIENT, DOCTOR, APPOINTMENT TABLE에서 2022년 4월 13일 취소되지 않은 흉부외과(CS)진료 예약 내역을 조회하는 SQL문을 작성해주세요. 

◾ 진료예약번호, 환자이름, 환자번호, 진료과코드, 의사이름, 진료예약일시 항목이 출력되도록 작성해주세요. 

◾ 결과는 진료예약일시를 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT A.APNT_NO, B.PT_NAME, B.PT_NO, A.MCDP_CD, C.DR_NAME, A.APNT_YMD 
    FROM APPOINTMENT A
        JOIN PATIENT B
            ON A.PT_NO = B.PT_NO
        JOIN DOCTOR C
            ON A.MDDR_ID = C.DR_ID
    WHERE A.MCDP_CD = 'CS'
        AND
            A.APNT_CNCL_YN = 'N'
        AND
            DATE_FORMAT(A.APNT_YMD, '%Y-%m-%d') = '2022-04-13'
    ORDER BY A.APNT_YMD ;

```
◾ 중요 포인트

    ▪ APPOINTMENT (A) : 진료예약번호(APNT_NO), 진료과코드(MCDP_CD), 진료예약일시(APNT_YMD)
    ▪ PATIENT (B) : 환자이름(PT_NAME), 환자번호(PT_NO),
    ▪ DOCTOR (C) : 의사이름 (DR_NAME)

    ▪ A.MCDP_CD = 'CS'
    ▪ A.APNT_YMD = '2022-04-13'

    ▪ APPOINTMENT.PT_NO = PATIENT.PT_NO
    ▪ APPOINTMENT.MDDR_ID = DOCTOR.DR_ID

    ▪ APPOINTMENT.APNT_YMD 오름차순 정렬 

◾ 세 테이블의 COLUMN이 필요하기 때문에 세 개의 테이블을 JOIN합니다. 
    ▪ A와 B는 PT_NO, A와 C는 DR_ID로 INNER JOIN 합니다. 
◾ WHERE절을 통해 예약이 취소되지 않은 흉부외과 진료 예약 내역을 필터링합니다. 
◾ APNT_YMD를 통해 정렬합니다. 

```