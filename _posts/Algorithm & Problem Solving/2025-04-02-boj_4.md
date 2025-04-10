---
title: "[백준_문제_풀이] 1535" 
categories: Algorithm&Problem_Solving
tag: [python, baekjoon, Coding_test]
---

## 문제 링크

[링크](https://www.acmicpc.net/problem/1535)

## 사용 알고리즘 

◾ 다이나믹 프로그래밍

◾ 브루트포스 알고리즘

◾ 배낭문제

## 풀이 
```python 
from itertools import combinations 

N = int(input())
health = list(map(int, input().split()))
happy = list(map(int, input().split()))

max_happy_sum = 0 

for i in range(0, N + 1) : 
    for combination in combinations(range(N), i) : 
        health_sum = 0
        happy_sum = 0 
        for j in combination : 
            health_sum += health[j]
            happy_sum += happy[j]
        
        if health_sum < 100 : 
            max_happy_sum = max(max_happy_sum, happy_sum)

print(max_happy_sum)
```

## 풀이과정

    ◾ N명 각각의 사람들과 인사를 1회 시행할 수 있습니다. 
    ◾ 인사를 시행하게 되면 체력이 health[i] 만큼 소모됩니다. 최대 체력은 100입니다. 
        ▪ 체력이 0이나 음수가 될 수 없습니다. 
    ◾ 대신 happy[i] 만큼 기쁨이 채워집니다. 
    ◾ 얻을 수 있는 최대 기쁨을 출력하는 문제입니다. 
    ◾ combination 으로 for 구문을 2회 돌림으로서 모든 부분집합을 비교하는 문제입니다. 
        ▪ for i in range(0, N + 1) → 0부터 N까지의 부분집합을 설계하기 위함입니다. 
        ▪ for combination in combinations(range(N), i) → 모든 부분집합의 인덱스를 가집니다. 
        ▪ 그러므로 combination을 health list와 happy list에서 인덱스를 더합니다. 
        ▪ max를 통해 부분집합마다 비교를 하며 최대로 기쁨을 느낄 수 있는 수치를 출력합니다. 