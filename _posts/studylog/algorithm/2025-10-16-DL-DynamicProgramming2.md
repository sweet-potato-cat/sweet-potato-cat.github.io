---
layout: post
title: "Dynamic Programming 2 Analysis"
subtitle: "Chain matmul, Optimal Binary Search Tree"
category: studylog
tags: algorithm
---

동적계획법2 연쇄행렬곱셈과 최적이진검색트리<br>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 연쇄행렬곱셈
**최적의 원칙 적용 여부**<br>
n개의 행렬을 곱하는 최적의 순서는 n개 행렬의 부분집합을 곱하는 최적의 순서를 포함

### 코드

```
def matrixmult(n, A, B):
    # Complete the code here
    C = [[0 for _ in range(n)] for _ in range(n)]
    for i in range(n):
        for j in range(n):
            for k in range(n):
                C[i][j] = C[i][j] + A[i][k] * B[k][j]
    return C
```
### 설계전략
가장 안쪽의 곱셈연산<br>
![code1](/assets/img/studylog/IMG_89546DCF9564-1.jpeg)

### 재귀관계식 적용 예
**$\` i \le k \le j-1 \`$**<br>
![code1](/assets/img/studylog/IMG_DCCEED64810C-1.jpeg)
**일정 시간복잡도**

$`T(n) = n^3$`<br>

## 1.5 binsearch_problem

### 코드
```
def binsearch(n, S, x):
    # Complete the code here
    low = 0 
    high = n-1
    location = -1
    
    if n == 0:
        return -1
    
    if x > S[high]:
        return -1
    elif x < S[low]:
        return -1
    
    while (low <= high):
        mid = (low + high) // 2
        if (x == S[mid]):
            location = mid
            return location
        elif (x < S[mid]):
            high = mid - 1
        else:
            low = mid + 1
        
    return location
```

### 단위연산
x와 S[mid]의 비교<br>

### 시간복잡도
입력값에 종속이다.<br>
![code2](/assets/img/studylog/IMG_A13BEF9C7B9F-1.jpeg)

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
for 문을 두 번 반복하니까 $`T(n) = n^2`$<br>
