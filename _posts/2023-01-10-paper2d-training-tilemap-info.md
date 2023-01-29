---
title: "[Paper2D Training] #8. 타일맵 정보 추출하기"
excerpt: "타일맵의 정보를 추출해 캐릭터 단위 이동시키기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, tilemap, map, info]

permalink: /paper2d-training/tilemap-info/

toc: true
toc_sticky: true

date: 2023-01-10 20:48:51+0900
last_modified_at: 2023-01-10 20:48:55+0900
---

## 👻 정보 추출하기
타일맵의 정보를 추출해서 캐릭터가 한 칸씩 이동할 수 있도록 할 것이다. 우선, 타일맵의 정보를 추출하려면 해당 타입을 상속받는 클래스를 하나 생성해줘야한다. 타일맵은 ``` Paper TileMap Actor ```를 상속받고 있으므로 해당 타입을 상속받는 블루프린트 클래스를 생성해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/paper-tilemap-actor.PNG)   

``` Render Component ```에 이전에 생성해두었던 ``` TileMap ```을 연결하면 정보를 추출할 준비가 되었다. 렌더 컴포넌트를 이용하면 타일맵에 관한 다양한 값을 추출할 수 있다.

***

### 🌱 배열화하기
2차원인 타일맵의 데이터를 1차원 배열로 관리할 것이다. 1차원 불리언 배열인 ``` Map Grid ```를 생성하고 클래스가 생성됨과 동시에 ``` Resize ``` 노드를 통해 배열의 크기를 정해주었다. 기본 세팅값은 False이다. 배열의 크기는 타일맵의 너비와 높이를 곱하여 구할 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/resize.PNG)   

***

### 🌱 값 세팅하기
``` Map Grid ```는 해당 인덱스에 속하는 타일에 벽이 있는지의 유무를 담고있는 배열이다. 인덱스는 좌에서 우로, 상에서 하로 증가하고 우측 끝에 다다르면 다음 줄의 가장 왼쪽으로 이동하여 반복한다. 벽이 있는지 확인하기 위해선 타일맵의 레이어1 즉, **Wall** 레이어를 확인한다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/set-array-elem.PNG)   

> 💡 **Get Tile**   
해당 좌표값에 타일의 유무를 판단하는 노드이다. 타일맵의 상세 페이지에서 각 타일에 마우스 오버하면 좌표값을 구할 수 있다. 좌표는 (X, Y) 값을 가리킨다.

인덱스에 **MOD 연산**을 하면 X 좌표의 값이, **나누기 연산**을 하면 Y 좌표의 값이 나온다는 규칙이 있으며 **벽이 있다면 True, 없다면 False를 저장**한다.

***

### 🌱 함수 만들기
좌표값을 인덱스로 변환해주는 ``` Grid Pos To Index ```, 좌표값을 받아 플레이어가 이동할 수 있는지 확인하기 위한 ``` Can Go ```, 해당 타일의 **월드 좌표**를 반환해주는 ``` Get Tile Pos ``` 함수를 만들었다.

- ``` Grid Pos To Index ```   
좌표값을 다시 인덱스로 바꾸는 공식은 다음과 같다.   

> Y 좌표의 값 * 맵의 너비 + X 좌표의 값

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/grid-pos-to-index.PNG)   

- ``` Can Go ```   
해당 좌표가 벽인지 아닌지 판단하는 함수이다.   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/can-go.PNG)   

- ``` Get Tile Pos ```   
맵의 좌표를 받아 해당 타일의 **월드 좌표**를 반환해주는 함수이다. **한 타일의 크기는 32x32 픽셀이고 스케일을 5로 늘려놓았기 때문에** 각 좌표값에 ``` 32 * 5 ``` 값을 곱해주었다. (0, 0)이 가장 왼쪽 상단에 위치한 타일이므로 아래로 갈수록 y 값은 줄어들기 때문에 5가 아닌 -5를 곱해야 한다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/get-tile-pos.PNG)   

***

## 👻 단위 이동하기
이제 캐릭터를 한 타일씩 이동하도록 만들어 볼 것이다. 그러기 위해선 플레이어가 **목적지에 대한 정보**를 가지고 있어야 한다. 목적지의 타일맵 좌표와 월드 좌표를 담는 변수를 새로 추가해주고 목적지를 정하는 함수를 추가해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/set-destination.PNG)   

목적지의 맵 좌표는 구조체로 관리하였고 월드 좌표는 벡터 변수로 관리하였다. 그리고 부드럽게 이동할지, 강제로 이동시킬지를 판단하는 ``` Force Move ``` 변수도 추가해주었다. 해당 변수가 True면 강제로 위치를 세팅시킨다.

또한 몬스터도 목적지를 가질 수 있기 때문에 ``` Creature ```에 추가해주었고, 플레이어인 ``` Knight ``` 클래스에서 게임이 시작될 때 목적지를 정하도록 ``` Set Destination ``` 함수를 호출해주었다. 참고로 첫 함수 호출은 곧 **스폰 좌표**를 지정하는 것과 동일하다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/first-set-destination.PNG)   

***

### 🌱 목적지 갱신
유저가 방향키를 눌렀을 때 호출 할 함수를 만들어야한다. 목적지의 정보를 갱신하는 ``` Update Destination ``` 함수를 만들어주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/update-destination.PNG)   

각 방향에 따라 알맞은 X, Y 방향 좌표를 설정하고 덧셈 연산을 통해 이동해야 할 목적지의 좌표를 세팅해준다. 해당 과정에서 다음에 향할 타일이 유효한지 ``` Can Go ``` 함수로 체크한다.

> ![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/next-xy.PNG)   
타일맵의 왼쪽 상단이 (0, 0)이고 우측으로 갈 수록 X 축 증가, 아래로 갈 수록 Y 축이 증가한다.

***

### 🌱 컨트롤러 수정
유저가 방향키를 입력할 때마다 목적지를 찾고 갱신해야하기 때문에 플레이어 컨트롤러의 코드를 수정해줘야한다. 방향키를 **짧게 눌렀다 뗐을 때** 목적지까지 간 후에 멈춰야 하고, **움직이는 동안은 계속 목적지가 갱신**되면서 이동해야한다. 컨트롤러를 수정하기 전에 ``` Creature ```의 ``` Update Logic ``` 함수를 수정해야한다. 해당 함수는 **상태가 Move일 때 실행되는 코드**로, 기존에는 제한없이 월드 좌표를 수정해 주었다면 이제는 타일 단위로 정확한 목적지의 월드 좌표가 존재하기 때문에 움직이는 동안 **목적지에 도착했는지 확인하는 함수** ``` Has Arrived To Dest ``` 를 활용해 분기를 나눠야한다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/update-logic.PNG)   

> 💡 ``` Has Arrived To Dest ```   
현재 월드 좌표와 목적지의 월드 좌표의 차를 구해 어느 정도 근접했다고 생각되는 거리값(아마도 픽셀 단위)을 기준으로 대소 비교를 하여 목적지에 도착했는지 아닌지를 판단한다.   
> 👉 함수에서 ``` Details 👉 Pure ```를 **True**로 설정하면 내용 변경 없이 사용만 하겠다는 뜻을 가지는 함수가 되며 초록색으로 변한다.

- **Move, Idle 상태 판단하기**   
상태 변경 시의 조건문을 수정해줘야한다. 기존에는 키를 누르고 있는 상태의 유무만 판단했는데 목적지 정보가 추가되면서 도착 유무까지 판단하도록 변경되었다. **목적지에 도착하지 않은 상태 AND 키를 누르지 않은 상태에도 상태는 Move로 유지된다.**

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/move-condition.PNG)   

- **Stop Move 변경하기**   
현재 키가 눌려져있는지 아닌지 판단하여 ``` Stop Move ```의 값을 설정해주는 조건문을 추가해주었다. 멈춰야한다면 목적지의 타일에 도달했을 때 캐릭터가 멈추게 될 것이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/set-stop-move.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-info/step.gif)   

***

## 👻 글을 마치며
이번 시간에는 타일맵의 정보를 추출하고 캐릭터를 타일 단위로 이동시켜보았다. 강의 내용이 양이 많아서 조금 어렵고 헷갈렸던 것 같다. 여러번 반복해서 보고 코드를 계속 들여다보니 이해는 됐지만 안 보고 만드려면 연습이 많이 필요할 것 같다. 수시로 코드를 리뷰해야겠다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/ji8q)_