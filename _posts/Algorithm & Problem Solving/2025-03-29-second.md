---
title: "[백준_문제_풀이] 2527" 
categories: Algorithm&Problem_Solving
tag: [python, baekjoon, Coding_test]
---

## 문제 링크

[링크](https://www.acmicpc.net/problem/2527)

## 사용 알고리즘 

- 기하학
- 많은 조건 분기 

## 풀이 
```python
answer = [
    ['d', 'd', 'd'], 
    ['d', 'c', 'b'], 
    ['d', 'b', 'a']
]

for _ in range(4) : 
    x1, y1, x2, y2, x3, y3, x4, y4 = map(int, input().split())

    # x축
    if x2 < x3 or x4 < x1 : 
        x_intersection = 0 
    elif x2 == x3 or x4 == x1 :
        x_intersection = 1
    else : 
        x_intersection = 2
    
    # y축
    if y2 < y3 or y4 < y1 : 
        y_intersection = 0 
    elif y2 == y3 or y4 == y1 : 
        y_intersection = 1 
    else : 
        y_intersection = 2

    print(answer[x_intersection][y_intersection])
```

## 풀이과정 

- x축과 y축의 경우의 수에 따라 나오는 정답을 답안으로 만들어 놓은 후, 축에 따라 결과를 만들고, 미리 만들어놓은 행렬에서 정답을 print 하는 문제입니다.