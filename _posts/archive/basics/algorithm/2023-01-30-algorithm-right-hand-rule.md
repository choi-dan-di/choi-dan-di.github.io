---
title: "[Algorithm] 오른손 법칙(Right-Hand Rule)"
excerpt: "오른손 법칙을 적용하여 미로의 입구에서 출구까지 길찾기"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, right hand rule, maze, find way]

permalink: /algorithm/right-hand-rule/

toc: true
toc_sticky: true

date: 2023-01-30 18:51:23+0900
last_modified_at: 2023-01-30 18:51:25+0900
---
 
## 👻 오른손 법칙
**오른손 법칙(Right-Hand Rule : 右手法)**은 미로에서 길을 찾을 때 사용하는 흔한 알고리즘이다. 바라보고 있는 방향에서 오른쪽 벽면만 타고 이동하는 법칙이다. 오른쪽으로 갈 수 있는지가 우선순위 중 가장 높고, 그 다음은 직진, 다음은 왼쪽 방향으로 회전하는 것이다.

***

### 🌱 미로 찾기
25 x 25 크기의 미로를 랜덤 생성하는 코드를 선행으로 적어주었고, **시작 지점은 (1, 1), 출구는 (24, 24)**로 지정해주었다.

먼저 미로가 생성되면, 플레이어를 생성하는데 동시에 미로의 길찾기도 진행된다. 경로를 벡터에 저장해 두었다가 일정 시간 단위로 경로따라 플레이어를 이동시켜주면 길찾기를 하는 것처럼 보여질 것이다.

- **다음 방향 포지션 찾기**   
```c++
Pos front[4] =
{
    Pos { -1, 0 },	// UP
    Pos { 0, -1 },	// LEFT
    Pos { 1, 0 },	// DOWN
    Pos { 0, 1 }	// RIGHT
};
```

``` Pos ```는 ``` (y, x) ```의 값을 가지는 구조체이다. 현재 플레이어의 진행 방향에 따라 다음 포지션 값을 구하기 위해 단위 포지션을 배열로 지정해주었다. **현재 포지션에 단위 포지션을 더해주면** 다음 포지션 값이 나오게 될 것이다.

- **목적지에 도착할 때까지 무한 루프 실행**   
```c++
while (pos != dest)
{
    // 1. 현재 바라보는 방향을 기준으로 오른쪽으로 갈 수 있는지 확인
    int32 newDir = (_dir - 1 + DIR_COUNT) % DIR_COUNT;
    if (CanGo(pos + front[newDir]))
    {
        // 오른쪽 방향으로 90도 회전
        _dir = newDir;

        // 앞으로 1보 전진
        pos += front[_dir];

        // 경로 저장
        _path.push_back(pos);
    }
    // 2. 현재 바라보는 방향을 기준으로 전진할 수 있는지 확인
    else if (CanGo(pos + front[_dir]))
    {
        // 앞으로 1보 전진
        pos += front[_dir];

        _path.push_back(pos);
    }
    else
    {
        // 왼쪽 방향으로 90도 회전
        _dir = (_dir + 1) % DIR_COUNT;
    }
}
```

진행 방향은 **UP, LEFT, DOWN, RIGHT** 순으로 반시계 방향으로 움직인다고 볼 수 있다. 열거형 방식을 사용해 지정해주었고 다음 진행 방향을 계산할 때 ``` if ~ else if ```나 ``` switch ```를 이용하는 대신 기존의 값들로만 계산하여 구해주었다. 다음 블럭이 갈 수 있는 블럭이라면 플레이어의 포지션을 해당 위치로 옮겨준 후 경로도 추가해주었다.

- **일정 시간마다 경로 업데이트**   
```c++
// 길찾기 알고리즘 구현
if (_pathIndex >= _path.size())
    return;

_sumTick += deltaTick;
if (_sumTick >= MOVE_TICK)
{
    _sumTick = 0;
    _pos = _path[_pathIndex];
    _pathIndex++;
}
```

대략적으로 0.1초 단위로 이동시켜주기 위해 ``` MOVE_TICK ```을 100으로 설정해주었고 델타틱을 더해주면서 계산하였다. 델타틱은 ``` ::GetTickCount364 ``` 함수를 사용하여 구해주었다. 해당 함수는 ms 단위의 시간값을 반환한다.

- **결과**   
![Alt Text](/assets/images/posts_img/basics/algorithm/right-hand-rule/right-hand-rule-result.gif)   

***

## 👻 글을 마치며
이번 시간에는 오른손 법칙의 개념과 해당 법칙을 활용해 미로에서의 길찾기를 간단하게 구현해보았다. 베이스가 없었을 때는 알고리즘 생각만 하면 막막했었는데 그래도 어느정도 기본 지식이 있으니 이해하기 훨씬 쉬웠고 혼자 만들어봤을 때 시간은 좀 걸렸지만 잘 만들 수 있었던 것 같다. 앞으로 다양한 알고리즘을 더 공부하면서 가장 좋은 나만의 알고리즘을 구현하는 것이 이번 공부의 목표이다.

***

_출처_   
_[인프런 Rookiss님 강의](https://inf.run/1JwV)_