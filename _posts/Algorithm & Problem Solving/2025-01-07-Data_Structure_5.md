---
title: "[Data_Structure] 큐" 
categories: Algorithm&Problem_Solving
tag: [python, Data_Structure, baekjoon, Coding_test, basic]
---

## 큐 (Queue)

### 큐의 정의와 기본 개념 

    ◾ 큐(Queue)는 선입선출 (First In, First Out, FIFO) 방식의 자료구조 입니다. 
        
        ▪ 따라서 먼저 들어온 데이터가 먼저 나가는 자료구조를 가집니다. 

        ▪ 예를 들어 음식점 번호표, 프린터 작업 대기열, BFS 등에 사용됩니다. 

### 큐의 특징 
    ◾ 선입선출 

        ▪ 먼저 들어간 데이터가 먼저 나옵니다. 

    ◾ 양 끝 조작

        ▪ 한쪽에서 넣고, 다른 한쪽에서 꺼내는 구조입니다. 

    ◾ Python 구현

        ▪ collections.deque 사용 (append, popleft 사용)

    ◾ 용도 

        ▪ BFS, 프로세스 스케줄링, 캐시 시스템 등이 있습니다. 

    ◾ 스택과 차이

        ▪ 스택은 후입선출, 큐는 선입선출 입니다. 

### 큐의 시간 복잡도 분석 

|Operation|Average case|
|-|-|
|append()|O(1)|
|popleft()|O(1)|
|peek()|O(1)|
|is_empty()|O(1)|
|len(queue)|O(1)|
|in|O(1) ~ O(n)|

### 예제코드 

[예제 18258](https://www.acmicpc.net/problem/18258)

```python
import sys
from collections import deque

n = int(input())
d = deque([])

for i in range(n) : 
    command = sys.stdin.readline()
    command = command.split()

    if command[0] == "push" : 
        d.append(command[1])
    
    if command[0] == "pop" : 
        if len(d) == 0 : 
            print("-1")
        else : 
            print(d.popleft())

    if command[0] == "size" : 
        print(len(d))

    if command[0] == 'empty' : 
        if len(d) == 0 : 
            print("1")
        else : 
            print("0")

    if command[0] == 'front' : 
        if len(d) == 0 :
            print(-1)
        else : 
            print(d[0])

    if command[0] == 'back' : 
        if len(d) == 0 : 
            print("-1")
        else : 
            print(d[-1])
```

    ◾ d는 빈 덱(deque), 큐로 사용할 변수입니다. 

    ◾ n은 명령어의 개수이고, 반복문으로 명령어를 처리합니다. 

        ▪ push X 

            ▫ 큐의 뒤쪽에 값 X를 추가합니다. 

        ▪ pop

            ▫ 큐에서 가장 앞의 값을 제거하고 출력합니다 (popleft())

            ▫ 큐가 비어있으면 -1을 출력합니다. 

        ▪ size 
            ▫ 큐에 들어있는 데이터의 갯수를 출력합니다. 

        ▪ empty 

            ▫ 큐가 비어있으면 1, 아니면 0을 출력합니다. 

        ▪ front 

            ▫ 큐에서 가장 앞의 값을 출력하지만 제거하지 않습니다. 

            ▫ 큐가 비어있으면 -1을 출력합니다. 

        ▪ back

            ▫ 큐의 가장 뒤에 있는 값을 출력하지만 제거하지 않습니다. 

            ▫ 큐가 비어있으면 -1을 출력합니다. 


[예제 2164](https://www.acmicpc.net/problem/4949)

```python
from collections import deque

n = int(input())
d = deque(list(range(1, n + 1)))

drop = True

while len(d) > 1 : 
    if drop : 
        d.popleft()
        drop = False
    else : 
        x = d.popleft()
        d.append(x)
        drop = True

print(d[0])    
```

    ◾ while문을 통해 카드가 한 장 남을 때까지 반복합니다. 

    ◾ 버릴 차례가 되면 popleft()를 통해 맨 앞 카드를 제거합니다.

    ◾ drop = False를 통해 다음엔 이동할 차례가 되도록 설정합니다. 

    ◾ 만약 drop = False라면 이동하는 차례이기 때문에 else에서 수행합니다. 

    ◾ popleft()를 통해 deque의 맨 앞 카드를 제거합니다. 

    ◾ 또한 append를 통해 deque의 맨 뒤에 카드를 추가합니다.

    ◾ 이동을 했다면 다시 카드를 버릴 차례이기 때문에 drop를 True로 설정합니다.

    ◾ 카드가 한 장이 남게 되었다면, while문을 빠져나오게 되고, d의 값을 출력하며 문제가 마무리 됩니다. 

### 큐의 사용 사례 

    ◾ 운영체제의 작업 스케줄링
    
        ▪ 프로세스 스케줄링

        ▪ 프린터 대기열 

    ◾ 데이터 스트리밍 처리 

    ◾ 웹 브라우저의 요청 처리 

    ◾ 시뮬레이션

    ◾ 메시지 큐 시스템(MQ)