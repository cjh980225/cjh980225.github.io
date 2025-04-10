---
title: "[백준_문제_풀이] 9935" 
categories: Algorithm&Problem_Solving
tag: [python, baekjoon, Coding_test]
---

## 문제 링크

[링크](https://www.acmicpc.net/problem/9935)

## 사용 알고리즘

◾ 자료 구조    
◾ 문자열   
◾ 스택   

## 풀이
```python
S = input()
T = input()

stack = [] 
for i in range(len(S)) : 
    stack.append(S[i])

    if len(stack) >= len(T) : 
        same = True 
        for j in range(len(T)) : 
            if stack[-len(T) + j] != T[j] : 
                same = False 
                break
        if same : 
            for _ in range(len(T)) : 
                stack.pop(-1)
if len(stack) == 0 : 
    print("FRULA")
else : 
    print("".join(stack))
```

## 풀이과정

    ◾ 첫번째 for문의 의미는 다음과 같습니다. 
        ▪ stack에 문자열 S를 한글자씩 append 합니다. 
        ▪ 만약 stack의 길이가 폭발 문자열 T의 길이보다 길어질 때마다 폭발을 시킬 수 있는 기회가 생성됩니다. 
        ▪ same = True로 기회를 부여하고, T의 길이만큼 반복문을 부여하여 stack과 T가 일치하는지 확인합니다. 
            ▫ stack[-len(T) + j] 는 stack의 길이가 T보다 길다는 조건 하에 stack의 T만큼 앞쪽을 의미합니다.
            ▫ 따라서 T의 길이만큼 반복문을 수행하였을 때, 전부 일치하지 않는다면 same을 False를 만들고 다시 진행합니다. 
        ▪ 만약 same이 여전히 True라면 T의 길이만큼 stack의 요소를 제거합니다. 
            ▫ -1부터 제거를 해도 일치하게 T의 길이만큼 제거하는 것이기 때문에 -1부터 T의 길이만큼 pop합니다. 
    ◾ 결과적으로 stack의 길이가 0이라면 "FRULA"를 출력하고, 0이 아니라면 공백없이 stack을 join하여 출력합니다. 