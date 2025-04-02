---
title: "[Algorithm] 정렬" 
categories: Algorithm&Problem_Solving
tag: [python, Algorithm, baekjoon, Coding_test, basic]
---

## 정렬 (Sorting)

## 정렬의 정의와 기본 개념 

    ◾ 원소들을 번호순이나 사전 순서와 같이 일정한 순서대로 열거하는 알고리즘이다. 

    ◾ 효율적인 정렬은 탐색이나 병합 알고리즘처럼 다른 알고리즘을 최적화하는데 중요하다. 

    ◾ 데이터의 정규화나 의미있는 결과물을 생성하는 데 유용히 쓰인다. 

## 정렬의 특징 

    ◾ 정렬 알고리즘의 종류 

        ▪ 버블 정렬 (Bubble sort)

        ▪ 선택 정렬 (Selection sort)

        ▪ 삽입 정렬 (Insertion sort)

        ▪ 병합 정렬 (Merge sort)

        ▪ 퀵 정렬 (Quick sort)

            ▫ 병합정렬과 퀵정렬은 복잡하지만 효율적인 정렬로 분류된다. 

            ▫ 하지만 모든 경우에 대해 최적의 성능을 보여주는 정렬 알고리즘은 존재하지 않는다. 
    
### 정렬의 시간 복잡도 분석 

|Operation|Best|Average|Worst|
|-|-|-|-|
|버블 정렬|O(n²)|O(n²)|O(n²)|
|선택 정렬|O(n²)|O(n²)|O(n²)|
|삽입 정렬|O(n)|O(n²)|O(n²)|
|병합 정렬|O(nlogn)|O(nlogn)|O(nlogn)|
|퀵 정렬|O(nlogn)|O(nlogn)|O(n²)|

### 예제코드 

[예제 1946](https://www.acmicpc.net/problem/1946)

```python
import sys 
input = sys.stdin.readline 

T = int(input())

for _ in range(T) :
    N = int(input())
    candidates = []

    for _ in range(N) : 
        s, m = map(int, input().split())
        candidates.append((s, m))

    # (s, m)
    candidates.sort()

    top_ranking = 1e9
    count = 0

    for i in range(N) : 
        if candidates[i][1] < top_ranking : 
            count += 1
            top_ranking = candidates[i][1]

    print(count)
```

    ◾ 다른 모든 지원자와 비교했을 때 서류심사 성적과 면접시험 성적 중 적어도 하나가 
        다른 지원자보다 떨어지지 않는 자만 선발합니다. 
    ◾ sys.stdin.readline을 통해 반복문으로 여러줄을 입력받을 때 시간초과가 발생할 수 있는 변수를 제거했습니다. 
    ◾ 변수 설명
        ▪ T = 테스트 케이스의 갯수 
        ▪ N = 케이스의 지원자 수 
        ▪ s = 지원자의 서류심사 성적
        ▪ m = 지원자의 면접심사 성적
        ▪ candidates = 지원자 list화 
    ◾ 선발할 수 있는 지원자를 count하기 위하여 input을 받고, sort를 통해 서류심사 성적으로 정렬을 합니다. 
    ◾ candidates[i][1]을 통해 서류심사 성적이 가장 높은 사람부터 평가하며 
        서류심사 성적과 면접심사 성적을 비교합니다. 
        ▪ 하위호환있지 않은 경우의 수를 색출합니다. 

[예제 11728](https://www.acmicpc.net/problem/11728)

```python
import sys 
input = sys.stdin.readline 

N, M = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))
C = []

pos1 = 0
pos2 = 0

while pos1 < N and pos2 < M : 
    candidate1 = A[pos1]
    candidate2 = B[pos2]

    if candidate1 < candidate2 : 
        C.append(candidate1)
        pos1 += 1
    else : 
        C.append(candidate2)
        pos2 += 1

if pos1 != N : 
    C.extend(A[pos1:N])
if pos2 != M :
    C.extend(B[pos2:M])

for i in range(N + M) : 
    print(C[i], end = " ")
```

    ◾ A와 B의 list를 합쳐서 C를 출력하는 문제입니다. 
    ◾ C는 정렬된 상태에서 print가 되어야 합니다. 
    ◾ pos1과 pos2를 len(A)와 len(B)가 되면 while문을 종료합니다. 
    ◾ A와 B를 번갈아 비교하며 순서대로 C list에 추가(append)하는 while문입니다. 
    ◾ 만약 A나 B의 리스트에 남는 수가 있게 되면 조건에 만족을 못하게 됩니다. 
    ◾ 따라서 if문을 통해 pos1과 pos2의 갯수가 N, M의 갯수와 다르게 된다면 extend를 통해 마지막까지 추가해주는 구문입니다. 
    ◾ C의 길이는 len(N) + len(M)이기 때문에 for문의 길이를 N+M으로 설정해주며, end를 통해 공백을 추가해줍니다.

### 참고

[정렬 위키피디아](https://ko.wikipedia.org/wiki%EC%A0%95%EB%A0%AC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

[길바닥 AI님의 블로그](https://gbdai.tistory.com/9)