---
layout: post
title: "Introduce Algorithm 1 Code Analysis"
subtitle: "Algorithm Ex1: 1.1 seqsearch, 1.2 arrsum, 1.3 exchangesort"
category: studylog
tags: algorithm
---

algorithm ex1.<br>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 1.1 seqsearch_problem.py

### 코드

```
def seqsearch(n, S, x):
    location = 0
    for i in range(n):
        if S[i] == x:
            return i
        if (i > n):
            return -1
    # Complete the code here
    

    return location
```
### 단위연산
x와 S[location]의 비교연산<br>

### 시간복잡도 분석

순차검색은 계산크기가 입력값에 종속되므로 일정 시간복잡도가 존재하지 않는다.<br>
**최악시간복잡도**<br>
배열에 원소가 없거나 마지막에 위치했을 때 W(n) = n<br>
**최선시간복잡도**<br>
배열의 맨앞에 원소가 위치할 때 B(n) = 1<br>
**평균시간복잡도**<br>
배열 안에 원소가 있을 확률을 p라고 하고, 원소가 k번째 있을 확률을 n이라고 두고 계산한다. 시그마로 이루어진 항은 원소가 있는 경우 n(1-p)는 배열에 원소가 없기 때문에 (1-p)계산을 n번 반복해주는 것이다.<br>
/assets/img/studylog/IMG_09482527BD8B-1.jpeg
따라서 $A(n) = n\left(1 - \frac{p}{2}\right) + \frac{p}{2}$ 이다.<br>

## 1.2 arrsum_problem

### 코드
```
def arrsum(n, S):
    result = 0
    # Complete the code here
    for i in range(n):
        result += S[i]

    return result
```

### 단위연산
덧셈연산 (비교나 할당이 아니다!)<br>

### 시간복잡도
입력값에 독립이고 $T(n) = n$<br>

## 1.3 exchangesort_problem

### 코드
```
def exchangesort(n, S):
    for i in range(len(S)):
        for j in range(i + 1, n):
            if S[i] > S[j]:
                S[i], S[j] = S[j], S[i]
        # Complete the code here
    return S
```
### 단위연산
S[i]와 S[j]의 비교연산 -> j 루프에서 실행된다<br>

### 시간복잡도 분석
교환정렬은 계산크기가 입력값에 독립이다.<br>
for 문을 두 번 반복하니까 $T(n) = n^2$<br>
