---
title: "[Algorithm] 프림(Prim) 알고리즘"
excerpt: "크루스칼과 비슷한 프림 알고리즘에 대해 알아보기"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, minimum spanning tree, tree, prim]

permalink: /algorithm/prim-algorithm/

toc: true
toc_sticky: true

date: 2023-02-15 21:05:28+0900
last_modified_at: 2023-02-15 21:05:29+0900
---
 
## 👻 프림 알고리즘
**프림 알고리즘(Prim Algorithm)**은 하나의 시작점으로 구성된 트리에 간선을 하나씩 수집하며 탐색을 진행하는 방식의 알고리즘이다. 기준 노드가 존재하기 때문에 각 노드와 연결된 간선만을 비교하여 트리를 확장해 나가는 방식이며 크루스칼 알고리즘과 마찬가지로 최소 스패닝 트리를 만드는 데 사용되는 알고리즘이다. 기준 노드가 있다는 점에서 다익스트라 알고리즘과 비슷하나 프림 알고리즘의 경우 기준 노드가 탐색할 때마다 변경된다는 차이점이 있다.

> 💡 **프림**은 **트리(정점 집합)**을 기준으로 최단 cost를 탐색하고, **다익스트라**는 변경되지 않는 **시작점**을 기준으로 최단 cost를 탐색한다.

***

### 🌱 맵 생성하기
오른손 법칙부터 시작해서 BST, 다익스트라, 크루스칼을 거쳐 만든 미로 찾기를 프림 알고리즘을 이용하여 구현해보자.

- **간선 정보를 담은 배열과 정점을 포함한 맵 생성하기**

```c++
struct CostEdge
{
    int cost;
    Pos vtx;

    bool operator<(const CostEdge& other) const
    {
        return cost < other.cost;
    }
};

for (int32 y = 0; y < _size; y++)
{
    for (int32 x = 0; x < _size; x++)
    {
        if (x % 2 == 0 || y % 2 == 0)
            _tile[y][x] = TileType::WALL;
        else
            _tile[y][x] = TileType::EMPTY;
    }
}

// edges[u] : u 정점과 연결된 간선 목록
map<Pos, vector<CostEdge>> edges;
```

프림 알고리즘은 기준이 되는 노드가 트리 단위로 진행되기 때문에 하나의 정점과 가중치 정보만 담는다. 그런 다음, 정점을 포함한 첫 단계의 맵을 생성해주고 간선의 정보를 포함하여 **기준이 되는 정점과 연결된 간선 목록을** 맵 형식으로 생성해주었다.

맵으로 생성하면 기준 정점을 키로 두고, 해당 정점과 연결된 간선 목록을 값으로 관리할 수 있게 된다.

- **간선에 랜덤 가중치값 등록하기**

```c++
for (int32 y = 0; y < _size; y++)
{
    for (int32 x = 0; x < _size; x++)
    {
        if (x % 2 == 0 || y % 2 == 0)
            continue;

        // 우측 연결하는 간선 후보
        if (x < _size - 2)
        {
            const int32 randValue = ::rand() % 100;
            Pos u = Pos{ y, x };
            Pos v = Pos{ y, x + 2 };
            edges[u].push_back(CostEdge{ randValue, v });
            edges[v].push_back(CostEdge{ randValue, u });
        }

        // 아래쪽 연결하는 간선 후보
        if (y < _size - 2)
        {
            const int32 randValue = ::rand() % 100;
            Pos u = Pos{ y, x };
            Pos v = Pos{ y + 2, x };
            edges[u].push_back(CostEdge{ randValue, v });
            edges[v].push_back(CostEdge{ randValue, u });
        }
    }
}
```

오른쪽과 아래쪽으로의 간선 후보들을 골라 랜덤으로 가중치를 지정해주었다. 양방향 연결을 위해 u와 v 모두 연결된 간선을 등록해주었다.

- **프림 알고리즘 구현**

```c++
// 해당 정점이 트리에 포함되어 있나?
map<Pos, bool> added;
// 어떤 정점이 누구에 의해 연결 되었는지
map<Pos, Pos> parent;
// 만들고 있는 트리에 인접한 간선 중, 해당 정점에 닿는 최소 간선의 정보
map<Pos, int32> best;

// 다익스트라랑 거의 유사
// 단, 다익스트라에서는 best가 [시작점]을 기준으로 한 cost
// 프림에서는 best가 [현재 트리]를 기준으로 한 간선 cost

for (int32 y = 0; y < _size; y++)
{
    for (int32 x = 0; x < _size; x++)
    {
        best[Pos{ y, x }] = INT32_MAX;
        added[Pos{ y, x }] = false; // 옵션
    }
}

priority_queue<CostEdge> pq;
const Pos startPos = Pos{ 1, 1 };   // 랜덤으로 정해도 됨
pq.push(CostEdge{ 0, startPos });
best[startPos] = 0;
parent[startPos] = startPos;

while (!pq.empty())
{
    CostEdge bestEdge = pq.top();
    pq.pop();
    
    // 새로 연결된 정점
    Pos v = bestEdge.vtx;
    // 이미 추가되었다면 스킵
    if (added[v])
        continue;

    added[v] = true;

    // 맵에 적용
    {
        int y = (parent[v].y + v.y) / 2;
        int x = (parent[v].x + v.x) / 2;
        _tile[y][x] = TileType::EMPTY;
    }

    // 방금 추가한 정점에 인접한 간선들을 검사한다
    for (CostEdge& edge : edges[v])
    {
        // 이미 추가 되었으면 스킵
        if (added[edge.vtx])
            continue;

        // 다른 경로로 더 좋은 후보가 발견 되었으면 스킵
        if (edge.cost > best[edge.vtx])
            continue;

        best[edge.vtx] = edge.cost;
        parent[edge.vtx] = v;
        pq.push(edge);
    }
}
```

우선순위 큐를 이용하여 구현해주었다. 다익스트라와 비슷하지만 기준이 되는 정점이 다르기 때문에 약간은 다른 계산 방식을 가진다. 아무래도 여러 간선을 비교해야 하다보니 맵을 이용하여 필요한 모든 정보를 저장하였다. 간선이라 지칭하는 칸은 정점을 사이에 두고 있기 때문에 두 정점의 좌표를 합친 수를 2로 나눈 값이 간선의 좌표값이 된다.

***

## 👻 글을 마치며
이번 시간에는 프림 알고리즘에 대해 알아보았다. 다익스트라와 비슷한 방식으로 만드는 최소 스패닝 트리 알고리즘같이 느껴졌다. 어느 상황에 크루스칼 혹은 프림을 써야할지는 조금 더 깊게 공부를 하고 두 알고리즘의 개념을 완벽히 이해한 후 찾아봐야 이해를 훨씬 수월하게 할 수 있을 것 같다.

***

_출처_   
_[인프런 Rookiss님 강의](https://inf.run/1JwV)_