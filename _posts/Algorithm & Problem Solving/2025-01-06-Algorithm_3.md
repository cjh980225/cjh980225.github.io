---
title: "[Algorithm] 완전탐색" 
categories: Algorithm&Problem_Solving
tag: [python, Algorithm, baekjoon, Coding_test, basic]
---

## 완전탐색 (Brute Force)

### 완전탐색의 정의와 기본 개념 

    ◾ Brute : 무식한, Force : 힘 따라서 무식한 힘으로 해석할 수 있습니니다. 

    ◾ 가능한 모든 경우의 수를 모두 탐색하면서 요구조건에 충족되는 결과만을 가져옵니다. 

    ◾ 일반적인 방법으로 문제를 해결하기 위해서는 모든 자료를 탐색해야 하기 때문에 특정한 구조를 전체적으로 탐색할 수 있는 방법을 필요로 합니다. 

    ◾ 알고리즘 설계의 가장 기본적인 접근 방법은 해가 존재할 것으로 예상되는 모든 영역을 전체 탐색하는 방법입니다. 

### 완전탐색의 특징 

    ◾ 완전탐색의 종류

        ▪ 선형 구조를 전체적으로 탐색하는 순차 탐색

        ▪ 비선형 구조를 전체적으로 탐색하는 깊이 우선 탐색 (DFS, Depth First Search)

        ▪ 비선형 구조를 전체적으로 탐색하는 너비 우선 탐색 (BFS. Breadth First Search)

    ◾ 완전탐색의 사용 조건

        ▪ 문제에서 달성하고자 하는 솔루션이 잘 정의되어 있어야 합니다. 

            ▫ 솔루션이 잘 정의되어 있지 않는 문제라면, 완전탐색을 사용한 솔루션이 올바른지의 여부를 확인할 수 없습니다. 

        ▪ 문제를 해결할 수 있는 풀이의 수가 제한되어 있어야 합니다. 

            ▫ 문제에서 고려해야 할 솔루션의 수가 한정되어 있어야 합니다. 고려해야 할 솔루션의 수가 무한하거나 너무 크면 효율적이지 않은 방법이 됩니다다. 
    

### 예제코드 

[예제 2309](https://www.acmicpc.net/problem/2309)

```python
from itertools import combinations

heights = []
for _ in range(9) : 
    height = int(input())
    heights.append(height)

for a in combinations(heights, 7) : 
    if sum(a) == 100 : 
        a = list(a)
        a.sort()
        for x in a : 
            print(x)
        break
```

    ◾ 9개의 줄에 걸쳐 난쟁이의 키가 주어집니다. 

    ◾ heights의 빈 list에 아홉 난쟁이의 키를 append 합니다. 

    ◾ combination을 통해 아홉 난쟁이 중 일곱 난쟁이를 추출하여 더합니다. 

    ◾ 만약 더한 값이 100이 된다면, 그 일곱 난쟁이가 정답이기에, sort를 통해 정렬합니다. 

    ◾ for 문을 통해 차례로 print 해주고, 여러 경우의 수가 나오는 것이기에 더이상의 경우의 수가 나오지 않도록 
        break를 해줍니다. 

    ◾ 결국 combination을 통해 ₉C₇ 로 모든 경우의 수를 생각하는 완전탐색이 됩니다. 

[예제 10819](https://www.acmicpc.net/problem/10819)

```python
from itertools import permutations

N = int(input())
A = list(map(int, input().split()))

max_diff_sum = 0

for a in permutations(A, N) : 
    diff_sum = 0
    for i in range(N - 1) : 
        diff_sum += abs(a[i] - a[i + 1])

    max_diff_sum = max(max_diff_sum, diff_sum)

print(max_diff_sum)
```

    ◾ N의 정수로 이루어진 배열 A에서 순서를 적절히 바꾸어서 최댓값을 구하는 프로그램입니다. 

        ▪ |A[0] - A[1]| + |A[1] - A[2]| + ... + |A[N-2] - A[N-1]|

    ◾ permutation을 통해 모든 순열을 생각하여 수식의 max 값을 출력합니다. 

### 참고

[정렬 위키피디아](https://ko.wikipedia.org/wiki%EC%A0%95%EB%A0%AC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

[길바닥 AI님의 블로그](https://gbdai.tistory.com/9)