---
layout: post
title: "딥러닝 1주차 과제"
subtitle: "1 Week Deep learning homework"
category: studylog
tags: deeplearning
---

딥러닝 1주차 과제 4문제.<br>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 1번

/assets/img/StudyLog/dlpost.png

## 2번

**재귀**나 **stack**을 이용하여 구현합니다.<br>
대부분의 경우 재귀 함수를 통해 구현하며, 이 경우 너무 깊게 탐색하게 될 경우 *stack memory limit*을 넘는 경우가 있을 수 있습니다.<br>
이 떄 도착점에 도달한 경로를 구했다 하더라도, 해당 경로가 **최단 경로라는 보장이 없습니다.**

### 구현

```c++
void dfs(int cur) {
  is_visited[cur] = true;
  cout << cur << " ";

  for (int next = 1; next <= total_node; next++)
    if (graph[cur][next] && !is_visited[next])
      dfs(next);
}
```

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
