---
title: "[Algorithm] 이분 탐색" 
categories: Algorithm&Problem_Solving
tag: [python, Algorithm, baekjoon, Coding_test, basic]
---

## 이분 탐색 (Binary Search)

### 이분탐색의 정의와 기본 개념 

    ◾ 이분 탐색 알고리즘은 정렬되어 있는 리스트에서 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 방법입니다. 

    ◾ 이분 탐색은 배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘 입니다. 

    ◾ 변수 3개 (start, end, mid)를 사용하여 탐색한다. 

        ▪ 찾으려는 데이터와 중간점 위치에 있는 데이터를 반복적으로 비교해서 원하는 데이터를 찾는 과정입니다. 

### 이분탐색의 특징 

    ◾ 검색 범위를 반으로 줄여서 탐색하므로 속도가 빠릅니다. 

    ◾ 검색 대상의 크기와 상관없이 빠른 검색 속도를 제공합니다. 

### 이분탐색의 시간 복잡도 분석 

|Operation|Best|Average|Worst|
|-|-|-|-|
|Search|O(1)|O(logN)|O(logN)|

### 예제코드 

[예제 2512](https://www.acmicpc.net/problem/2512)

```python
N = int(input())
jibang = list(map(int, input().split()))
budget = int(input())

left = 0
right = max(jibang)
answer = -1 

while left <= right : 
    middle = (left + right) // 2

    sum = 0 
    for i in range(N) : 
        sum += min(middle, jibang[i])

    if sum <= budget : 
        answer = middle 
        left = middle + 1
    else : 
        right = middle -1

print(answer)
```

    ◾ 변수 설명
        ▪ N : 지방의 수 
        ▪ jibang : 지방에서 요청하는 예산 금액
        ▪ budget : 총 예산 금액 
    ◾ left = 0 과 right = jibang 의 최댓값을 통해 middle = 중앙값을 산출했습니다. 
    ◾ 최솟값과 최댓값이 만나는 값에서 while을 종료시킵니다. 
        ▪ sum 값에 middle 값과 jibang 인덱스 값들 중 최소를 sum에 추가합니다. 
        ▪ sum 값은 결국 지방에 지원할 수 있는 총 예산 금액을 이야기 하는 것입니다. 
        ▪ 따라서 sum 값과 budget 값을 비교합니다. 
            ▫ sum <= budget 이라면 지방에 예산을 더 지원할 수 있으므로 left를 이동합니다. 
            ▫ left를 이동하며 answer 을 middle 증가시킵니다. 
            ▫ sum > budget 이라면 지방에서 요청하는 금액이 총 예산 금액을 뛰어넘는 것입니다. 
            ▫ 따라서 right 값을 이동하며 middle 값을 감소시킵니다. 
    ◾ 따라서 answer은 각 지방에게 지원할 수 있는 금액의 최대를 출력합니다. 
        ▪ answer 보다 jibang[i]의 값이 작다면 jibang[i]의 값을 지원합니다. 
        ▪ answer 보다 jibang[i]의 값이 크다면 answer의 값을 지원합니다. 
        
[예제 2343](https://www.acmicpc.net/problem/2343)

```python
N, M = list(map(int, input().split()))
running_time = list(map(int, input().split()))

# 이분 탐색 
left = max(running_time)
right = sum(running_time)
answer = -1 

while left <= right : 
    middle = (left + right) // 2 # 임시 블루레이 용량 

    blueray_num = 1
    remain = middle 

    for i in range(N) : 
        if remain < running_time[i] : 
            blueray_num += 1
            remain = middle 

        remain -= running_time[i]
    
    if blueray_num <= M : 
        answer = middle 
        right = middle - 1 # [left, middle - 1]
    else : 
        left = middle + 1 # [middle + 1, right]

print(answer)
```

    ◾ 변수 설명
        ▪ N : 강의의 수
        ▪ M : 블루레이 수 
        ▪ running_time : 강의의 길이가 강의 순서대로 분 단위 list 
    ◾ 강의를 블루레이에 저장하고, 가능한 블루레이 크기 중 최소를 출력하는 문제입니다. 
    ◾ left는 running_time의 최댓값, right는 running_time의 총계로 지정합니다. 
    ◾ for i in range(N) 구문은 blueray 1개의 임시 용량을 middle 값으로 지정합니다. 
    ◾ 이 후 blueray_num이 몇 개가 소모되는지 체크를 합니다. 
        ▪ blueray의 갯수보다 M개가 더 적으면 blueray의 갯수가 부족한 것이므로 middle 값을 증가시킵니다. 
        ▪ blueray의 갯수보다 M이 더 많거나 같으면 최소 answer은 middle이거나 더 적은 값이 될 수 있습니다.
    ◾ 따라서 left 값과 right 값이 만나는 지점에서 while문이 종료되고, answer 값을 출력합니다.     

### 참고

[kdb.velog 님의 블로그](https://velog.io/@kimdukbae/%EC%9D%B4%EB%B6%84-%ED%83%90%EC%83%89-%EC%9D%B4%EC%A7%84-%ED%83%90%EC%83%89-Binary-Search)


[yoongrammer 님의 블로그](https://yoongrammer.tistory.com/75)