---
title: "[Data_Structure] 스택" 
categories: Algorithm&Problem_Solving
tag: [python, Data_Structure, baekjoon, Coding_test, basic]
---

## 스택 (Stack)

### 스택의 정의와 기본 개념 

    ◾ 스택(Stack)은 "쌓다"라는 의미로 데이터를 하나씩 쌓아 올린 형태의 자료구조입니다. 

    ◾ 한쪽 끝에서만 자료를 넣고 뺄 수 있는 LIFO(Last In First Out) 형식의 자료구조입니다. 

        ▪ 박스 쌓기를 생각하면 쉽습니다. 

            ▫ 박스를 아래에서 하나씩 쌓고, 박스를 뺄 때는 마지막에 쌓은 박스부터 다시 순서대로 빼야 한다는 점을 생각하면 쉽습니다. 
    
    ◾ 스택은 완전히 꽉 찼을 때 Overflow 상태라고 하며 완전히 비어 있으면 Underflow상태라고 합니다. 

    ◾ 삽입 (Push)와 제거(Pop)은 모두 Top이라는 스택의 한쪽 끝에서만 일어납니다. 

### 스택의 특징 

    ◾ push - 스택 맨 위에 항목을 삽입

    ◾ pop - 스택 맨 위에 항목 삭제 

    ◾ peek or top - 스택의 맨 위(top)을 표시 

    ◾ isEmpty - 스택이 비어있는지 확인
    
    ◾ isFull - 스택이 가득 차 있는지 확인

    ◾ getSize - 스택에 있는 요소 수를 반환


### 스택의 시간 복잡도 분석 

|Operation|Average case|Worst case|
|-|-|-|
|Access|O(n)|O(n)|
|Search|O(n)|O(n)|
|Insert(push)|O(1)|O(1)|
|Delete(pop)|O(1)|O(1)|

### 예제코드 

[예제 10773](https://www.acmicpc.net/problem/10773)

```python
K = int(input())

stack = []

for i in range(K) : 
    n = int(input())

    if n == 0 : 
        stack.pop(-1)
    else : 
        stack.append(n)

sum = 0 
for e in stack : 
    sum += e

print(sum)
```

    ◾ K개의 줄을 for 문을 통해 stack에 쌓으며, stack의 합을 출력하는 문제입니다. 

    ◾ 빈 stack을 만들어주고 for문을 통해 stack을 채워줍니다. 

    ◾ 만약 0이라면 최근에 쓴 수를 지우고 아닐 경우 해당 수를 채워줍니다. 

    ◾ sum을 0으로 설정하고, for 문을 통해 stack의 요소를 sum에 차례차례 더해주고 print 해줍니다. 

[예제 4949](https://www.acmicpc.net/problem/4949)

```python
while True : 
    s = input()
    if s == "." : 
        break 

    stack = []
    balanced = True

    for i in range(len(s)) : 
        if s[i] == '(' or s[i] == '[' : 
            stack.append(s[i])

        if s[i] == ")" : 
            # stack 이 비어있으면 안됨 
            if len(stack) == 0 : 
                balanced = False
                break 

            last = stack.pop(-1)
            if last != '(' : 
                balanced = False 
                break 

        if s[i] == "]" : 
            # stack 이 비어있으면 안됨 
            if len(stack) == 0 : 
                balanced = False
                break 

            last = stack.pop(-1)
            if last != '[' : 
                balanced = False 
                break 

# 마지막에는 stack이 비어 있어야 함 
    if len(stack) != 0 : 
        balanced = False 

    if balanced == True : 
        print('yes')
    else : 
        print('no')
```

    ◾ while문을 통해 먄약 s 가 .이라면 while문을 종료합니다. 
    
    ◾ 빈 스택을 만들어주고 s의 길이로 for문을 돌려주며, 만약 "(" 이나 "[" 가 나오면 stack에 추가합니다. 

    ◾ 이 후, ")"이 나오면, s의 길이를 확인하고, 0이 아니라면 "(" 인지 확인합니다. 

        ▪ 만약 "("이 맞다면 진행, 아니라면 balance = False 입니다. 

    ◾ 이 후, "]"이 나오면, s의 길이를 확인하고, 0이 아니라면 "[" 인지 확인합니다. 

        ▪ 만약 "["이 맞다면 진행, 아니라면 balance = False 입니다. 

    ◾ 마지막의 stack은 비어있어야 하며, balance가 여전히 True라면 balance가 잘 맞는 것이니 yes를 출력합니다. 
    
    ◾ 이 외의 상황은 balance가 달라졌으므로, no를 출력합니다. 

### 스택의 사용 사례 

    ◾ 재귀 알고리즘을 반복문을 통해서 구현할 수 있게 해줍니다. 

    ◾ 실행 취소 (undo)

    ◾ Backtracking

    ◾ 웹 브라우저 뒤로가기

    ◾ 구문분석

    ◾ 후위(postfix) 표기법 연산

    ◾ 문자열의 역순 출력 등

### 참고

[아이딕 IT블로그님의 블로그](https://tmdrnr96.tistory.com/28)

[yoongrammer 블로그](https://yoongrammer.tistory.com/45)