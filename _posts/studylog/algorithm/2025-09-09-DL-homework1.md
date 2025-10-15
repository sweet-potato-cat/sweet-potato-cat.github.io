---
layout: post
title: "Introduce Algorithm1"
subtitle: "Algorithm Ex1"
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



`dfs()` 재귀 호출을 통해 방문한 노드를 표시해주고, 포문을 통해 연결된 노드 중 아직 방문하지 않은 노드를 재귀 호출합니다.

## 3번

**queue**를 이용해 간단하게 구현할 수 있습니다.<br>
또한 **가중치가 없는 그래프**에서는 도착점까지의 **최단 경로**를 구할 수 있습니다.<br>

### 구현

```c++
void bfs(int start) {
  queue<int> myQ;
  is_visited[start] = true;
  myQ.push(start);

  while(!myQ.empty()) {
    int cur = myQ.front();
    myQ.pop();
    cout << cur << " ";
    for (int next = 1; next <= total_node; next++) {
      if (graph[cur][next] && !is_visited[next]) {
        is_visited[next] = true;
        myQ.push(next);
      }
    }
  }
}
```
