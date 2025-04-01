---
title: "[Data_Structure] 배열" 
categories: Algorithm&Problem_Solving
tag: [python, Data_Structure, baekjoon, Coding_test, basic, list]
---

## 배열 (list)

### 배열의 정의와 기본 개념 

    ◾배열은 연속된 메모리 공간에 순차적으로 저장된 데이터 모음입니다. 

    ◾배열을 구성하는 각각의 값을 요소 (element)라고 하며, 배열에서의 위치를 가리키는 숫자는 인덱스 (index)라고 합니다. 

### 배열의 특징

    ◾ 배열의 각 요소에 접근하는 시간은 O(1)로 모두 동일합니다. 

    ◾ 연속된 메모리에 단일 블록화하여 데이터를 저장합니다. 

    ◾ 실제 메모리 상에서 물리적으로 데이터가 순차적으로 저장되기 때문에 데이터에 순서가 있으며, index가 존재하여 indexing 및 slicing이 가능합니다. 


### 배열의 시간 복잡도 분석 

|Operation|Average case|Worst case|
|-|-|-|
|read|O(1)|O(1)|
|insert|O(n)|O(n)|
|delete|O(n)|O(n)|
|search|O(n)|O(n)|

### 예제 코드 

[예제 5597](https://www.acmicpc.net/problem/5597)

```python
n_list = []
not_in_list = []

for i in range(1, 29) : 
    x = int(input(""))
    n_list.append(x)

for i in range(1, 31) : 
    if i not in n_list : 
        not_in_list.append(i)
    else : 
        pass
        
not_in_list.sort()

print(not_in_list[0])
print(not_in_list[1])
```

    ◾30명의 학생 중 과제를 제출하지 않은 2명을 찾는 문제입니다. 
    
    과제를 제출한 학생의 list를 먼저 작성하고 이후 작성하지 않은 학생들의 list를 작성하여 sort()를 
    
    사용하여 정렬하고 출석번호가 작은 학생을 먼저 출력하는 코드를 작성하였습니다. 

[예제 3052](https://www.acmicpc.net/problem/5597)

```python
remain_list = []

for i in range(10) : 
    number = int(input())
    number_remain = number%42

    if number_remain not in remain_list : 
            remain_list.append(number_remain)

print(len(remain_list))
```

    ◾10개의 수를 42로 나누어 나머지를 중복없이 list에 append 하여 remain_list의 길이를 출력하는 문제입니다. 

### 참조 

[yoongrammer님의 블로그](https://yoongrammer.tistory.com/43)
