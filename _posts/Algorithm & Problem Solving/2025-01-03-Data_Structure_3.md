---
title: "[Data_Structure] 문자열" 
categories: Algorithm&Problem_Solving
tag: [python, Data_Structure, baekjoon, Coding_test, basic]
---

## 문자열 (String)

### 문자열의 정의와 기본 개념 

    ◾ 둘 이상의 결합된 문자을 뜻합니다. 문자열은 단순구조입니다. 
        
        ▪ 단순구조 : 정수, 실수, 문자, 문자열 등 자료의 형태 
    
        ▪ 구조는 단순구조, 선형구조, 비선형구조, 파일구조 등이 있습니다. 

### 문자열의 특징 

    ◾ 데이터 구성
        
        ▪ 문자열은 문자 시퀀스로, 이를 통해 텍스트 데이터를 구조화된 방식으로 표현할 수 있습니다. 

        ▪ 이러한 구성은 단어와 문장과 같은 텍스트 정보를 처리하기 쉽게 해줍니다. 
    
    ◾ 고정 또는 동적 길이 
        
        ▪ 문자열은 고정 길이이거나 동적 길이일 수 있어서, 데이터를 저장하고 조작하는 방법에 유연성을 부여할 수 있습니다. 

    ◾ 연산

        ▪ 문자열은 연걸, 슬라이싱, 검색, 변환과 같은 다양한 연산을 지원합니다. 

        ▪ 이러한 연산을 통해 프로그래머는 문자열 데이터를 효율적으로 조작할 수 있습니다.  

    ◾ 메모리 관리

        ▪ 문자열은 메모리에서 연속된 문자 블록으로 관리될 수 있으며, 이를 통해 효율적인 엑세스와 수정이 가능합니다. 

        ▪ 이는 문자열 조작에 크게 의존하는 애플리케이션의 성능에 특히 증요합니다. 

    ◾ 알고리즘에서의 사용 

        ▪ 검색, 정렬, 패턴 매칭과 같은 많은 알고리즘은 문자열을 기본 데이터 구조로 활용합니다. 

        ▪ 이는 컴퓨터 과학과 프로그래밍에서 문자열의 역할을 더욱 강조합니다. 

    ◾ 복잡한 구조의 표현현
        
        ▪ 문자열은 JSON이나 XML과 같은 애플리케이션에서 데이터 교환에 사용되는 
            보다 복잡한 데이터 구조를 나타낼 수 있습니다. 

### 예제코드 

[예제 10809](https://www.acmicpc.net/problem/10809)

```python
s = input()

check = [-1] * 26

for i in range(len(s)) : 
    index = ord(s[i]) - ord('a')

    if check[index] == -1 : 
        check[index] = i 

for i in range(26) : 
    print(check[i], end = " ")
```

    ◾ 단어 s의 각각 알파벳에 대하여 다음과 같은 조건을 만족하는 결과값을 print합니다. 
        
        ▪ 단어에 포함되어 있는 경우에는 처음 등장하는 위치 출력 

        ▪ 단어에 포함되어 있지 않는 경우에는 -1을 출력

    ◾check는 알파벳의 위치를 나타내기 위해 길이가 26인 list를 만들었습니다.

    ◾ len을 통해 단어 s의 처음부터 끝까지의 길이를 for문을 통해 반복합니다. 

        ▪ ASCII 코드를 통해 글자의 위치를 찾습니다. 

            ▫ 'b' - 'a' = 1 / 'c' - 'a' = 2 ••• 'z' - 'a' = 25

        ▪ 만약 -1 이라면 처음 나온 글자이기 때문에 i를 통해 위치를 추가합니다. 

    ◾ for 문을 통해 check의 전문을 출력하고 중간 공백을 위해 end = " " 을 추가합니다.

[예제 11720](https://www.acmicpc.net/problem/11720)

```python
length = int(input())
num = input()

sum = 0

for i in range(length) : 
    sum = sum + int(num[i])

print(sum)
```

    ◾ N개의 숫자가 공백 없이 쓰여있는데, 이 숫자를 모두 합해서 출력하는 프로그램을 작성하는 문제입니다. 

    ◾ 처음 입력받는 num을 int 형식이 아닌 string 형식으로 받습니다. 

    ◾ for문을 돌며 num의 인덱스를 따로 출력하며 이 때 int형식으로 변환하여 더해줍니다. 

### 참고

[Quora](https://www.quora.com/Why-is-a-string-a-data-structure)

[Jin_dooly.log님의 블로그](https://velog.io/@jin-dooly/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EB%AC%B8%EC%9E%90%EC%97%B4String)