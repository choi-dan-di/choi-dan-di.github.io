---
title: "[Algorithm] 다익스트라 알고리즘(Dijkstra Algorithm)"
excerpt: "다익스트라 알고리즘에 대해 알아보기"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, nonlinear, graph, dijkstra]

permalink: /algorithm/dijkstra-algorithm/

toc: true
toc_sticky: true

date: 2023-02-02 19:45:03+0900
last_modified_at: 2023-02-02 19:45:06+0900
---
 
## 👻 다익스트라 알고리즘
**다익스트라 알고리즘(Dijkstra Algorithm)**이란 그래프 내의 노드 간 최단 경로를 구하는 알고리즘이다. BFS를 기반으로 진행되지만 탐색 순서가 다를 수 있다는 차이점이 있다. 가중치를 계산하여 시작 노드부터 도착 노드까지의 최단 경로와 비용을 알 수 있다. 다음 코드는 다익스트라 알고리즘을 이용하여 0번 노드부터 모든 노드 간의 가중치를 구하는 기능을 구현한 것이다.

- **코드**   

```c++
void Dijkstra(int here)
{
    struct VertexCost
    {
        int vertex;
        int cost;
    };

    list<VertexCost> discovered;    // 발견 목록
    vector<int> best(vertices.size(), INT32_MAX); // 각 정점별로 지금까지 발견한 최소 거리
    vector<int> parent(6, -1);

    discovered.push_back(VertexCost{ here, 0 });
    best[here] = 0;
    parent[here] = here;

    while (!discovered.empty())
    {
        // 제일 좋은 후보를 찾음
        list<VertexCost>::iterator bestIt;
        int bestCost = INT32_MAX;

        for (auto it = discovered.begin(); it != discovered.end(); ++it)
        {
            if (it->cost < bestCost)
            {
                bestCost = it->cost;
                bestIt = it;
            }
        }

        int cost = bestIt->cost;
        here = bestIt->vertex;
        discovered.erase(bestIt);

        // 더 짧은 경로를 뒤늦게 찾았다면 스킵
        if (best[here] < cost)
            continue;

        // 방문
        for (int there = 0; there < 6; there++)
        {
            // 연결되지 않았으면 스킵
            if (adjacent[here][there] == -1)
                continue;

            // 더 좋은 경로를 과거에 찾았으면 스킵
            int nextCost = best[here] + adjacent[here][there];
            if (nextCost >= best[there])
                continue;

            discovered.push_back(VertexCost{ there, nextCost });
            best[there] = nextCost;
            parent[there] = here;
        }
    }
}
```

***

### 🌱 코드 분석

- **선언부**

```c++
struct VertexCost
{
    int vertex;
    int cost;
};

list<VertexCost> discovered;    // 발견 목록
vector<int> best(vertices.size(), INT32_MAX); // 각 정점별로 지금까지 발견한 최소 거리
vector<int> parent(6, -1);
```

우선 현재 노드에서 목적지 노드까지의 비용을 정점과 함께 관리하기 위해 구조체를 만들었다. 일반적인 BFS와는 다르게 발견 목록을 큐로 관리할 수 없다. 발견한 노드와 탐색한 노드의 순서가 다를 수 있기 때문이다. 또한 최소 비용(거리)을 저장할 수 있는 동적 배열과 부모 노드의 경로를 알 수 있는 동적 배열을 선언해주었다.

- **초기화**

```c++
discovered.push_back(VertexCost{ here, 0 });
best[here] = 0;
parent[here] = here;
```

- **최단 거리 찾기**

```c++
// 제일 좋은 후보를 찾음
list<VertexCost>::iterator bestIt;
int bestCost = INT32_MAX;

for (auto it = discovered.begin(); it != discovered.end(); ++it)
{
    if (it->cost < bestCost)
    {
        bestCost = it->cost;
        bestIt = it;
    }
}
```

이 코드부턴 발견된 노드들이 전부 탐색될 때까지 무한 반복된다. 이터레이터를 이용해 지정한 노드에서 해당 노드까지 가장 적은 비용이 드는 경로를 저장해 줄 것이다. 발견한 노드 중 더 적은 비용이 드는 노드를 베스트로 지정한다.

> 💡 그래프의 크기가 아주 크거나 최악의 경우 모든 노드를 순회한다고 가정하면 위의 코드는 절대 좋은 코드가 아니다. 아무튼 가장 좋은 후보만 찾으면 되기 때문에 이러한 기능을 가지는 자료 구조를 사용하는 것이 훨씬 효율적일 것이다.   
>
> ``` priority_queue ``` 같은 경우 큐에 들어간 정보 중 가장 좋은(후보에 알맞는) 노드를 한 번에 찾아 주기 때문에 위와 같은 조건에 사용하면 훨씬 효율적일 것이다.

- **다음 노드 선정**

```c++
int cost = bestIt->cost;
here = bestIt->vertex;
discovered.erase(bestIt);
```

최적의 경로를 찾았다면 다음 노드로 변경한다. 발견 목록에선 삭제하며 더 이상 탐색 후보에 오르지 않도록 한다.

```c++
// 더 짧은 경로를 뒤늦게 찾았다면 스킵
if (best[here] < cost)
    continue;
```

현재 ``` best[here] ```은 내가 있는 노드(목적지)의 최소 비용을 의미한다. 이 비용이 내가 위에서 찾은 비용보다 더 적다면 아래의 코드는 무시(스킵)한다.

- **노드 탐색**

```c++
// 방문
for (int there = 0; there < 6; there++)
{
    // 연결되지 않았으면 스킵
    if (adjacent[here][there] == -1)
        continue;

    // 더 좋은 경로를 과거에 찾았으면 스킵
    int nextCost = best[here] + adjacent[here][there];
    if (nextCost >= best[there])
        continue;

    discovered.push_back(VertexCost{ there, nextCost });
    best[there] = nextCost;
    parent[there] = here;
}
```

``` here ```노드와 연결되어있는 ``` there ```노드를 모두 찾는다. 연결되어있지 않으면 스킵하고, 연결되어있다면 그 노드와 연결되어있는 간선의 가중치(비용)를 구한다. 비용은 출발 노드(0번)에서 해당 노드까지의 총합을 의미한다. 만약 이 비용이 최소 비용보다 크다면 스킵한다.

문제가 없다면 ``` discovered ```에 새 구조체를 만들어 추가하고 최소 비용 배열의 값을 수정해준다. 부모 노드 정보를 저장하는 배열도 수정해준다. 이러한 과정을 모든 노드를 탐색할 때까지 진행하고 프로그램은 종료된다.

***

## 👻 글을 마치며
이번 시간에는 다익스트라 알고리즘에 대해 알아보았다. 학부 시절에 잠시 봤었던 알고리즘인데 그 당시에는 이해하기가 어려웠었다. 이론과 프로그래밍은 많이 다르니 말이다. 하지만 이번엔 이론을 프로그래밍화 시키는 법도 점점 이해하기 쉬워졌고 공부를 할수록 스킬이 향상되는 것 같은 느낌과 동시에 그리 어렵지 않게 이해하고 구현할 수 있었다. 이 감을 잃지 않도록 꾸준히 익혀야 할 것 같다.

***

_출처_   
_[인프런 Rookiss님 강의](https://inf.run/1JwV)_