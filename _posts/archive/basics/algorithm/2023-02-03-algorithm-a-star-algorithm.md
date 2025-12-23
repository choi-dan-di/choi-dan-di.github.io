---
title: "[Algorithm] A* 길찾기 알고리즘"
excerpt: "A* 알고리즘을 이용해 미로에서 최단 경로로 길찾기 개선하기"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, a* algorithm, a star]

permalink: /algorithm/a-star-algorithm/

toc: true
toc_sticky: true

date: 2023-02-03 20:28:56+0900
last_modified_at: 2023-02-03 20:29:00+0900
---
 
## 👻 A* Algorithm
**A*(A Star, 에이 스타) 알고리즘**은 다익스트라 알고리즘과 같은 최단 경로로 길을 찾는 알고리즘이다. 하지만 다익스트라 알고리즘과는 다르게 **시작 노드**와 **목적지 노드**를 분명하게 알고있어야 하고 두 노드에서 모두 거리 계산을 하게된다. 이전에 만들어 놓은 미로 길찾기 알고리즘을 개선해보자.

***

### 🌱 코드 분석
A* 알고리즘은 각 노드 간의 점수를 계산해 해당 점수가 낮은 순으로 순위를 매긴다. 최종 점수는 **시작 노드에서 해당 노드까지 이동하는 데 드는 비용 G**와 **목적지에서 얼마나 가까운지를 체크하는 H**를 합산하여 구한다.

- **시작 노드와 목적지 노드 정하기**

```c++
Pos start = _pos;
Pos dest = _board->GetExitPos();
```

A* 알고리즘을 이용하려면 **시작 노드**와 **목적지 노드**를 알아야한다.

- **탐색 방향 정하기**

```c++
enum
{
	DIR_COUNT = 8
};

Pos front[] =
{
	Pos { -1, 0 },	// UP
	Pos { 0, -1 },	// LEFT
	Pos { 1, 0 },	// DOWN
	Pos { 0, 1 },	// RIGHT
	Pos { -1, -1 },	// UP_LEFT
	Pos { 1, -1 },	// DOWN_LEFT
	Pos { 1, 1 },	// DOWN_RIGHT
	Pos { -1, 1 },	// UP_RIGHT
};

int32 cost[] =
{
	10,	// UP
	10,	// LEFT
	10,	// DOWN
	10,	// RIGHT
	14,	// UP_LEFT
	14,	// DOWN_LEFT
	14,	// DOWN_RIGHT
	14	// UP_RIGHT
};

const int32 size = _board->GetSize();
```

앞서 만들었던 알고리즘에서는 확인 방향을 상하좌우 총 4방향으로만 설정해주었는데 이번엔 대각 방향까지 지정해주었다. 또한 A* 알고리즘을 사용하려면 각 방향에 위치한 노드까지의 이동하는데 소요되는 비용을 설정해주어야 하기 때문에 ``` cost ```라는 배열을 따로 만들어 비용을 정하였다. 기존에 설정해두었던 ``` DIR_COUNT ```도 따로 빼주어 관리하였다.

- **Open List와 Closed List**

```c++
vector<vector<bool>> closed(size, vector<bool>(size, false));
vector<vector<int32>> best(size, vector<int32>(size, INT32_MAX));
map<Pos, Pos> parent;
priority_queue<PQNode, vector<PQNode>, greater<PQNode>> pq;
```

``` closed ```라는 이차 배열을 하나 생성해줘서 해당 노드를 방문했는지 여부를 저장한다. 방문 유무만 저장하기 때문에 이 배열은 **클로즈 리스트(Closed List)**에 속한다. 클로즈 리스트는 방문 여부만을 저장하는 목적을 가진다.

``` best ```는 해당 노드까지 이동했을 때 가장 적게 소모된 비용만을 저장하는 배열이다. 뒤늦게 더 적은 비용을 가지는 경로를 발견했을 때를 대비해 최상의 조건을 유지하기 위해 사용한다.

``` parent ```는 부모 노드의 정보를 저장하기 위해 사용한다. 해당 맵을 통해 최단 경로를 역으로 추적하여 구할 수 있다.

``` priority_queue ```, 즉 **우선순위 큐**는 **오픈 리스트(Open List)**로 클로즈 리스트와는 다르게 **발견한 노드들의 정보**를 저장한다. 아직 탐색 전 노드들이라는 것에 주의해야한다. 해당 큐에는 노드의 좌표 뿐만 아니라 A* 알고리즘에 사용할 **최종 점수 F, 거리 비용 G**도 함께 저장된다.

> 💡 **F = G + H**   
- **F** : A* 알고리즘을 이용하여 계산된 최종 점수
- **G** : 시작점에서 해당 좌표까지 이동하는데 드는 비용
- **H** : **[휴리스틱(Heuristics)](https://ko.wikipedia.org/wiki/%ED%9C%B4%EB%A6%AC%EC%8A%A4%ED%8B%B1_%EC%9D%B4%EB%A1%A0) 추정값**의 약자. 목적지에서 얼마나 가까운지 체크하기 위해 사용하는 비용 계산 공식. 대표적으로 **피타고라스 정리** 법칙을 이용하지만 경우에 따라 자유롭게 설정할 수 있다.

- **초기값 설정**

```c++
int32 g = 0;
int32 h = 10 * (abs(dest.y - start.y) + abs(dest.x - start.x));
pq.push(PQNode{ g + h, g, start });
best[start.y][start.x] = g + h;
parent[start] = start;
```

시작 노드에서 출발하기 때문에 초기 비용은 0이다. 휴리스틱 추정값은 **[Y 좌표의 차이 + X 좌표의 차이]** 값에 10을 곱해주는 방식으로 구해주었다.

그런 다음 오픈 리스트(우선순위 큐)에 해당 좌표에 대한 정보를 저장해주면서 시작 노드를 발견 리스트에 추가해 관리한다.

``` best ```에는 방금 계산한 F값을 저장해주고 부모 노드 정보는 자기 자신으로 세팅해준다.

> ``` PQNode ```   
> 
> ```c++
> struct PQNode
> {
> 	bool operator<(const PQNode& other) const { return f < other.f; }
> 	bool operator>(const PQNode& other) const { return f > other.f; }
> 
> 	int32	f;	// f = g + h
> 	int32	g;
> 	Pos		pos;
> };
> ```
>
> 최종 점수의 대소 비교를 위해 부등호 연산을 재정의해주었다.

***

여기서부터 오픈 리스트에 데이터가 없을 때까지 무한 반복하며 실행되는 코드이다.

- **제일 좋은 후보 찾기**   

```c++
PQNode node = pq.top();
pq.pop();
```

우선순위 큐를 사용했기 때문에 가장 앞에 있는 노드를 꺼내면 최소 비용을 소모하는 노드가 추출될 것이다.

- **예외 처리하기**   
동일한 좌표를 여러 경로로 찾았을 때, 더 빠른 경로로 인하여 이미 방문(closed)된 경우 스킵해야한다. 이 때, 위에서 만들었던 배열 중 ``` closed ```를 사용하거나 ``` best ```를 사용하는 것은 자유이다.

```c++
// [선택]
// 1. closed 사용
if (closed[node.pos.y][node.pos.x])
    continue;
// 2. best 사용
if (best[node.pos.y][node.pos.x] < node.f)
    continue;
```

- **해당 노드 방문**

```c++
closed[node.pos.y][node.pos.x] = true;

// 목적지에 도착했으면 바로 종료
if (node.pos == dest)
    break;
```

클로즈 리스트의 정보를 변경해주어 노드 방문 상태를 변경해주었다. 해당 노드의 현재 좌표가 목적지와 동일하다면 반복문은 즉시 종료된다.

- **다음 진행 노드 탐색**

```c++
for (int32 dir = 0; dir < DIR_COUNT; dir++)
{
    Pos nextPos = node.pos + front[dir];
    // 갈 수 있는 지역은 맞는지 확인
    if (!CanGo(nextPos))
        continue;
    // [선택] 이미 방문한 곳이면 스킵
    if (closed[nextPos.y][nextPos.x])
        continue;

    // 비용 계산
    int32 g = node.g + cost[dir];
    int32 h = 10 * (abs(dest.y - nextPos.y) + abs(dest.x - nextPos.x));
    // 다른 경로에서 더 빠른 길을 찾았으면 스킵
    // 동일하면 먼저 세팅된 best 값 사용
    if (best[nextPos.y][nextPos.x] <= g + h)
        continue;

    // 예약 진행
    best[nextPos.y][nextPos.x] = g + h;
    pq.push(PQNode{ g + h, g, nextPos });
    parent[nextPos] = node.pos;
}
```

8방향으로 주변 노드를 탐색한다. 먼저 **갈 수 있는 노드인지 탐색**하고 다음으로 **이미 방문한 노드라면 스킵**하는 예외 처리까지 해주었다.

해당 구간을 무사히 통과했다는 것은 갈 수 있는 노드 + 아직 방문하지 않은 노드라고 볼 수 있다. 이제 휴리스틱 추정값을 이용하여 비용을 계산해준다. **G**는 현재 노드의 G값과 다음 노드로 가는 간선의 비용 ``` cost[dir] ```을 더해준다.

이렇게 구한 비용이 이미 이전에 발견되어 구해진 비용(이 있을 때)보다 **낮다면 정보를 갱신해주고** 다음 노드로의 설정을 진행한다.

위의 과정을 반복하면 오픈 리스트에 있는 노드 중에서 F값이 가장 적은 노드 순으로 뽑히게 될 것이며 다음 노드로 설정되고, 최단 경로를 가지는 길찾기가 진행될 것이다.

- **결과**   
![Alt Text](/assets/images/posts_img/basics/algorithm/a-star-algorithm/a-star-algorithm-result.gif)   

***

## 👻 글을 마치며
이번 시간에는 A* 알고리즘에 대해 알아보았다. 처음 들어보는 길찾기 알고리즘이었는데 코드 상으로 다익스트라와 크게 달라진 점은 없었던 것 같고 오히려 휴리스틱 추정값이라는 것을 이용해 계산을 하니 조금 더 정교해진 느낌이 들었다. 우선순위 큐를 사용하니 훨씬 구현이 편리해졌고 따로 구글링을 통해 여러 자료를 찾아보면서 보다보니 금세 이해할 수 있었던 것 같다. 여러 이론들을 공부하면서 동시에 내가 게임 개발을 할 때 어느 기능에 이러한 이론을 적용시켜야할지 동시에 생각하다보니 동기부여도 더욱 잘 되는 것 같다.

***

_출처_   
_[인프런 Rookiss님 강의](https://inf.run/1JwV)_

_참고하면 좋을 게시글_   
_[최단 경로 탐색 – A* 알고리즘](http://www.gisdeveloper.co.kr/?p=3897)_