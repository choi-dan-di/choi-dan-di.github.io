---
title: "[Paper2D Training] #4. 캐릭터 이동하기"
excerpt: "방향키에 따른 캐릭터 이동시키기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, animation, move character, move]

permalink: /paper2d-training/move-character/

toc: true
toc_sticky: true

date: 2023-01-07 20:52:55+0900
last_modified_at: 2023-01-07 20:52:59+0900
---

## 👻 캐릭터 이동하기
지난 시간까지 방향키에 따른 애니메이션을 설정하고 갱신했었다. 이번 시간에는 진짜로 캐릭터가 이동하는 기능을 구현해 볼 것이다.

***

### 🌱 Set Actor Location
가장 간단한 방법은 ``` Set Actor Location ``` 노드를 이용하는 것이다. 액터의 로케이션 정보를 세팅해주는 노드이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/move-character/set-actor-location.PNG)   

역시나 스피드와 델타 세컨드를 이용해 x 축으로만 이동하는 기능을 구현하였다.

> **Event Tick**도 수정해주었다.   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/move-character/event-tick.PNG)   

***

### 🌱 Character Movement
Paper Character를 상속받음으로써 생성된 ``` Character Movement ```를 이용하여 위와 같은 기능을 구현해보자. 위에선 위치만 지정해줬다면 해당 기능은 캐릭터의 전반적인 기능을 모두 포함한다. 걷는 속도부터 가속도, 중력, 관성 등 여러가지의 물리 법칙을 각각 컨트롤 할 수 있고 심지어 하늘을 나는 조건을 포함해 자유자재로 캐릭터를 이동시킬 수 있게 해준다.

대표적으로 ``` Add Input Vector ```와 ``` Add Movement Input ``` 노드가 있고 두 노드는 큰 차이가 없다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/move-character/add-input-vector.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/move-character/add-movement-input.PNG)   

``` Add Movement Input ``` 노드를 이용하여 좌우로 움직인다면 x 축 방향으로, 상하로 움직인다면 z 축 방향으로 움직이게끔 설정해주었다.

- **결과**   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/move-character/move2.gif)   

이 외에도 우측에 많은 항목이 있고, 하나하나씩 변경하고 테스트해보며 각각 어떤 기능을 가지는지 익혀야 할 것 같다.

***

## 👻 글을 마치며
이번 시간에는 캐릭터를 이동시키는 방법에 대해 알아보았다. 언리얼 엔진 에디터 기능을 많이 알면 알 수록 코드가 짧아지는 느낌이 든다. 그만큼 어려워지지만 😭 그래도 어느정도 감이 잡히니 쉽게 따라할 수 있었고 금세 만들 수 있었던 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/ji8q)_