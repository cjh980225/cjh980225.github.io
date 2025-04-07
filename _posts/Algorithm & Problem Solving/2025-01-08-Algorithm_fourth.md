---
title: "[Algorithm] 투포인터" 
categories: Algorithm&Problem_Solving
tag: [python, Algorithm, baekjoon, Coding_test, basic]
---

## 투포인터 (Two Pointer)

### 투포인터의 정의와 기본 개념

    ◾ 투 포인터는 두 개의 포인터(인덱스)를 사용해서 배열이나 리스트를 효율적으로 탐색하는 알고리즘 기법입니다. 

    ◾ 보통은 정렬된 배열이나 부분 구간을 다룰 때 많이 사용되며, 이중 반복문을 한 번의 순회로 최적화할 수 있는 테크닉 입니다. 

    ◾ 대표적인 구조 

        ▪ 두 포인터는 보통 start와 end 또는 left와 right로 표현됩니다.

        ▪ 같은 방향으로 이동하거나, 서로 반대 방향에서 좁혀오기도 합니다. 

    ◾ 기본 사용 예 

        ▪ 합이 특정 값이 되는 쌍 찾기 

        ▪ 구간의 최대합, 최소합 찾기

        ▪ 중복 없이 가장 긴 부분 문자열 찾기

        ▪ 슬라이딩 윈도우 기법과 함께 사용 

### 투포인터의 특징

    ◾ 시간 효율성 

        ▪ O(N²) → O(N) 또는 O(log N) 으로 최적화 가능

    ◾ 연속 구간 탐색에 적합

        ▪ 부분합, 구간 최대 / 최소 같은 연속 데이터 처리에 매우 강력

    ◾ 정렬된 배열에서 효과적

        ▪ 정렬된 데이터에서 두 값을 비교하며 이동하기 쉬움

    ◾ 구현이 직관적

        ▪ 단순한 반복문과 조건문으로 구현 가능

    ◾ 다양한 패턴

        ▪ 좌우에서 좁히는 방식, 한 방향으로 같이 이동하는 방식 등 다양 

### 예제코드

[예제 2003](https://www.acmicpc.net/problem/2003)

```python
N, M = list(map(int, input().split()))
A = list(map(int, input().split()))

start = 0 # 시작점
end = 0 # 끝 점
sum = A[0]

count = 0
while True : 
    # 현재 구간 합이 M인지 확인
    if sum == M : 
        count += 1 

    # 구간을 업데이트 
    if sum >= M : 
        start += 1
        sum -= A[start - 1]
    else : # sum < M
        if end == N - 1 : 
            break

        end += 1
        sum += A[end]

print(count)
```

    ◾ 정수 N개로 이루어진 수열 A가 주어집니다.

    ◾ 이 수열에서 연속된 부분 수열들의 합이 M이 되는 경우가 몇 개인지 구하는 문제입니다.

    ◾ start = end = 0 을 통해 구간을 이동시키는 포인터를 생성합니다. 

    ◾ sum = A[0]을 통해 현재 구간의 합을 구합니다.

    ◾ count = 0 을 통해 조건을 만족하는 구간의 수를 count합니다. 

    ◾ 현재 구간의 합이 M과 같으면? 

        ▪ 정답으로 인정하고 count += 1 을 수행합니다.

    ◾ 현재 구간의 합이 M보다 크거나 같으면? 

        ▪ 구간의 앞부분을 줄이기 위해 start를 오른쪽으로 이동합니다.

        ▪ 줄인 값은 합에서 빼줍니다.

    ◾ 현재 구간의 합이 M보다 작으면? 

        ▪ 값이 더 커질 수 있게 end를 오른쪽으로 이동합니다.

        ▪ 새로운 값을 sum에 추가합니다. 

    ◾ 더 이상 end를 움직일 수 없을 때 반복을 종료합니다. 

[예제 7795](https://www.acmicpc.net/problem/7795)

```python
T = int(input())

for _ in range(T) : 

    N, M = list(map(int, input().split()))

    A = list(map(int, input().split()))
    A.sort()

    B = list(map(int, input().split()))
    B.sort()

    main = 0
    sub = 0
    count = 0

    while True : 
        if sub == M or main == N:
            break 
        
        else : 
            if A[main] > B[sub] : 
                count += N - main
                sub += 1

            else : 
                main += 1
            
    print(count)
```

    ◾ main은 A 배열의 포인터, sub는 B 배열의 포인터, count는 정답을 세는 변수입니다. 

    ◾ main이 A 배열의 끝에 도달하거나 B 배열의 끝에 도달하면 비교가 불가능하기 때문에 루프를 종료합니다. 

    ◾ A가 B보다 큰 쌍의 개수를 출력하는 것이기 때문에 A[main] > B[sub] 의 부분을 찾고, count를 해줍니다. 

        ▪ sort를 활용하여 정렬을 해놓았기 때문에 전체 다 돌지 않아도 N - main으로 숫자를 파악할 수 있습니다. 

    ◾ A[main]이 B[sub]보다 작거나 같다면 A쪽을 한 칸 이동해서 다음 숫자와 비교합니다. 

    ◾ 이후 count를 출력하면 A가 B보다 큰 쌍의 갯수를 출력할 수 있습니다. 33