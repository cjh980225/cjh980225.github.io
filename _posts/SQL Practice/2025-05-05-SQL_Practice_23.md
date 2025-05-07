---

title: "[Programmers_SQL] 인기있는 아이스크림"


categories: SQL_Practice


tag: [SQL, Programmers, Solve]

---

## 문제 링크

LV 1️⃣ 

[링크](https://school.programmers.co.kr/learn/courses/30/lessons/133024)

## TABLE 설명

◾ FIRST_HALF

|Column name|Type|Nullable|Info|
|-|-|-|-|
|`SHIPMENT_ID`|INT(N)|FALSE|아이스크림 공장에서 아이스크림 가게까지의 출하 번호|
|`FLAVOR`|VARCHAR(N)|FALSE|아이스크림 맛|
|`TOTAL_ORDER`|INT(N)|FALSE|상반기 아이스크림 총주문량|


## 문제 설명

◾ 상반기에 판매된 아이스크림의 맛을 총주문량을 기준으로 내림차순 정렬해주세요. 

◾ 총주문량이 같다면 출하 번호를 기준으로 오름차순 정렬해주세요. 

## solution.sql
    SELECT FLAVOR
    FROM FIRST_HALF
    ORDER BY TOTAL_ORDER DESC, SHIPMENT_ID ASC;

```
◾ FIRST_HALF TABLE에서 FLAVOR을 정렬해서 출력합니다. 

◾ TOTAL_ORDER 기준으로 내림차순, SHIPMENT_ID를 기준으로 오름차순임으로 순서에 맞게 작성합니다. 
```