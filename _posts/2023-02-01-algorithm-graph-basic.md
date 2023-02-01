---
title: "[Algorithm] 그래프(Graph)"
excerpt: "비선형 자료 구조인 그래프에 대해 알아보기"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, nonlinear, graph]

permalink: /algorithm/graph-basic/

toc: true
toc_sticky: true

date: 2023-02-01 20:08:51+0900
last_modified_at: 2023-02-01 20:08:55+0900
---
 
## 👻 그래프
**그래프(Graph)**는 정점과 간선으로 이루어진 비선형 자료 구조 중의 한 종류이다. 현실 세게의 사물이나 추상적인 개념 간의 연결 관계를 표현한다고 볼 수 있다. 소셜 네트워크와 같은 **관계도 등**에 활용된다.

![Alt Text](/assets/images/posts_img/basics/algorithm/graph-basic/graph-example.png)   

> - **정점(Vertex)** : 데이터를 표현 (사물, 개념 등)
- **간선(Edge)** : 정점들을 연결하는 데 사용
    - 간선이 숫자를 가지는 **가중치 그래프(Weighted Graph)**가 있다.
    - 간선이 방향을 가지는 **방향 그래프(Directed Graph)**가 있다.
    
간선이 숫자를 가지는 **가중치 그래프(Weighted Graph)**와 방향을 가지는 **방향 그래프(Directed Graph)**가 있다.

***

## 👻 탐색하기
그래프가 주어질 때 탐색하는 방법에 대해 알아보자. 탐색 방법에는 **깊이 우선 탐색(DFS : Depth First Search)**과 **너비 우선 탐색(BFS : Breadth First Search)**이 있다.

***

### 🌱 DFS
**깊이 우선 탐색(DFS : Depth First Search)**은 그래프의 깊이를 우선시하는 탐색 방법이다. 루트 노드에서 탐색을 시작했을 때 다음 노드로 넘어가고, 해당 노드에서 또 연결된 노드가 있는지 탐색한다. 리프 노드에 다다랐을 때 다시 뒤로 돌아오며 길이 있으면 해당 노드로 넘어가 다시 탐색한다. 재귀함수를 이용하여 구현할 수 있다.

- **코드**   

```c++
void Dfs(int here)
{
    // 방문
    visited[here] = true;
    cout << "Visited : " << here << endl;

    // 길이 있는지 확인
    // 1. 인접 리스트 버전
    // 모든 인접 정점을 순회한다.
    for (int i = 0; i < adjacent[here].size(); i++)
    {
        int there = adjacent[here][i];

        if (!visited[there])
            Dfs(there);
    }

    // 2. 인접 행렬 버전
    for (int there = 0; there < vertices.size(); there++)
    {
        // 연결이 되어있지 않음
        if (adjacent2[here][there] == 0)
            continue;

        // 아직 방문하지 않은 곳에 있으면 방문한다.
        if (!visited[there])
            Dfs(there);
    }
}

// 끊어져있는 노드가 있을 때 탐색이 되지 않는 경우 방지
void DfsAll()
{
    visited = vector<bool>(6, false);

    for (int i = 0; i < vertices.size(); i++)
        if (!visited[i])
            Dfs(i);
}

int main()
{
    CreateGraph();

    DfsAll();
}
```

- **결과**   
``` 0 👉 1 👉 2 👉 3 👉 4 👉 5 ```

***

### 🌱 BFS
**너비 우선 탐색(BFS : Breadth First Search)**은 깊이 우선 탐색과는 반대로 루트 노드에서 가까운 거리순으로 탐색한다. 큐를 이용하여 탐색할 대기열을 설정할 수 있고 발견하는 노드와 탐색하는 노드의 실행 차이가 존재한다.

- **코드**   

```c++
void Bfs(int here)
{
    // 누구에 의해 발견 되었는지?
    vector<int> parent(6, -1);
    // 시작점에서 얼만큼 떨어져 있는지?
    vector<int> distance(6, -1);

    // 발견한 노드 목록 저장
    queue<int> q;
    q.push(here);
    discovered[here] = true;
    
    parent[here] = here;
    distance[here] = 0;

    while (!q.empty())
    {
        here = q.front();
        q.pop();

        // 방문
        cout << "Visited : " << here << endl;

        // 인접한 노드 찾기
        for (int there : adjacent[here])
        {
            if (discovered[there])
                continue; 

            q.push(there);
            discovered[there] = true;

            parent[there] = here;
            distance[there] = distance[here] + 1;
        }
    }
}

void BfsAll()
{
    discovered = vector<bool>(6, false);

    for (int i = 0; i < vertices.size(); i++)
    {
        if (!discovered[i])
            Bfs(i);
    }
}

int main()
{
    CreateGraph();

    BfsAll();
}
```

- **결과**   
``` 0 👉 1 👉 3 👉 2 👉 4 👉 5 ```

***

## 👻 글을 마치며
이번 시간에는 비선형 자료 구조 중의 하나인 그래프에 대해 알아보았고 DFS, BFS까지 간단하게 훑어보았다. 속성으로 코테 준비할 때 탐색 방법을 머릿속에서만 알고 있고 프로그래밍 코드로는 전혀 구현하지 못했었는데 이번 기회에 확실하게 알게 되어서 알고리즘 문제를 풀 수 있을 것 같다. 공부한 범위라면 이제 매일 한 번씩은 알고리즘 문제를 푸는 게 좋을 것 같다.

***

_출처_   
_[인프런 Rookiss님 강의](https://inf.run/1JwV)_