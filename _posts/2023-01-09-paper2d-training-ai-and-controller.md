---
title: "[Paper2D Training] #6. AI를 통해 몬스터 움직이기"
excerpt: "인공지능과 컨트롤러에 대해 알아보고 사용해보기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, ai, fsm, controller]

permalink: /paper2d-training/ai-and-controller/

toc: true
toc_sticky: true

date: 2023-01-09 23:04:04+0900
last_modified_at: 2023-01-09 23:04:08+0900
---

## 👻 FSM
이전에 만들었던 몬스터에 인공지능을 추가하여 자동으로 움직이는 기능을 구현해보려한다. 원래는 언리얼 엔진 자체에 **Behaviour Tree**라는 것을 사용하면 만들 수 있는데 이번 시간에는 FSM을 통해 직접 구현해 볼 것이다.

**FSM**은 **유한 상태 기계(Finite State Machine)**의 약자로써 게임 AI에게 지능을 부여하기 위한 하나의 모델이다. **State 패턴**을 통하여 쉽게 구현할 수 있다.

> 💡 **State 패턴이란?**   
말 그대로 상태에 따른 패턴을 구현하면 게임 AI가 자동으로 움직일 수 있도록 해주는 방법이다. 현재 개발중인 프로젝트에 비유하면 Idle, Move, Skill 상태에 따라 각기 다른 행동을 할 수 있도록 각 상태에 실행할 코드를 구현해 둔 것이다.

***

### 🌱 AI 만들어보기
간단하게 AI 기능을 구현해보자. ``` Monster ```에 ``` Update AI ``` 함수를 만들어 구현해 줄 것이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/event-tick.PNG)   

매 프레임마다 상태를 확인하여 행동하도록 ``` Event Tick ``` 부분에 함수를 붙여주었다. 해당 함수에서는 플레이어와 자기 자신 간의 벡터 차를 이용해 단위 벡터를 구하고, 플레이어 방향으로 이동하며 범위 내에 들어오면 공격 이벤트를 실행하는 기능을 구현할 것이다.

***

#### 🪐 Idle
우선 몬스터가 **대기 상태**일 때는 플레이어의 액터 정보를 가져와 ``` Target Enemy ``` 변수에 세팅하고 상태(State)를 ``` Move ```로 바꾸는 기능을 한다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/idle-state.PNG)   

***

#### 🪐 Move
몬스터가 **움직이는 상태**일 때는 플레이어와 자기 자신 간의 단위 벡터를 이용해 방향을 정한 후 움직이며, 일정 범위 내에 플레이어가 들어오면 ``` Begin Attack ```을 실행하여 공격하는 기능을 한다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/move-state.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/move-state2.PNG)   

좌우와 상하를 비교하여 조금 더 멀리 있는 축을 우선으로 움직이도록 기준을 정해주었다.

***

## 👻 Controller
이제 컨트롤러를 이용해 플레이어 및 몬스터가 움직이는 기준을 정해주자. 앞서 설정했던 ``` Player 0 ```이 바로 플레이어 컨트롤러이다. 컨트롤러는 게임 캐릭터와 유저 간의 연결을 해주는 기능을 한다. 유저가 직접 조작하여 캐릭터를 움직일 수 있게 해주는 것이 바로 컨트롤러의 역할이다.

**블루프린트 클래스**에서 ``` Player Controller ```를 상속받아 플레이어 컨트롤러를 만들 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/player-controller.PNG)   

몬스터에 적용시켜 줄 ``` AI Controller ```도 상속받아 컨트롤러를 하나 추가해주었다. 해당 컨트롤러는 잘 사용되지 않아 검색을 통해 찾을 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/ai-controller.PNG)   

마지막으로 게임의 전반적인 규칙, 점수 등을 설정할 수 있는 **게임 모드**도 하나 추가해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/game-mode-base.PNG)   

***

### 🌱 Player
플레이어 컨트롤러에 ``` Update Input ``` 함수를 이전시켜주었다. 이제 플레이어가 움직이는 기능을 담당하는 해당 함수를 컨트롤러로 따로 빼서 관리할 것이기 때문이다. 플레이어가 오로지 하나만 존재한다면 굳이 필요없겠지만 여럿이 동시에 존재할 수 있기 때문에 따로 묶어 관리하면 편리하다. 컨트롤러와 연결할 플레이어 설정은 다음과 같다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/my-player.PNG)   

> 💡 **Get Controlled Pawn**   
현재 컨트롤러가 지정한 폰을 가져온다.

***

### 🌱 Monster
몬스터는 AI 컨트롤러로 따로 빼서 지정해주었다. 해당 컨트롤러는 ``` Update AI ``` 함수를 가짐으로써 게임 AI가 지능을 가지고 이동하는 것과 같은 전반적인 움직임을 담당할 것이다. 현재 컨트롤러가 담당하고 있는 몬스터를 가져오는 방법은 플레이어와 동일하다. 추가로 몬스터의 상세 페이지에서 **폰의 AI Controller Class**는 방금 만든 몬스터용 컨트롤러를 연결시켜야한다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/monster-ai-controller.PNG)   

***

### 🌱 Game Mode
마지막으로 만들어 둔 게임 모드 하나를 우리가 지정한 플레이어 컨트롤러를 연결하여 새로운 게임 모드를 하나 만들어주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/game-mode.PNG)   

만든 게임 모드를 설정하면 다음과 같이 기본으로 세팅된 값이 나오고, 여기에서 ``` Player Controller Class ```와 ``` Default Pawn Class ```를 우리가 만들어 둔 클래스로 세팅하면 완성된다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/bp-player-controller.PNG)   

인게임에 존재했던 기존의 플레이어를 삭제시킨 후 ``` Player Start ```를 이용해 스폰 지점을 따로 지정해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/player-start.PNG)   

- **결과**

![Alt Text](/assets/images/posts_img/projects/paper2d-training/ai-and-controller/chasing.gif)   

***

## 👻 글을 마치며
이번 시간에는 몬스터에 AI를 적용시켜보고 컨트롤러까지 설정해보았다. 도대체 게임에서 몬스터가 자연스레 움직일까에 대해 항상 많은 궁금증이 있었는데 오늘 공부로 말끔히 해결된 것 같다. 오늘은 아주 간단하게 인공지능의 아주아주아주 기초적인 부분을 직접 구현해보았지만 기존에 존재하는 인공지능 기능을 사용하면 더욱 다양한 이벤트를 만들어낼 수 있을 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/ji8q)_