---
title: "[3D Game Training] #1. 캐릭터 생성 & 이동하기"
excerpt: "애셋을 다운받아 적용하고 캐릭터 생성하고 이동시켜보기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, character, pawn]

permalink: /3d-game-training/create-character/

toc: true
toc_sticky: true

date: 2023-01-17 21:48:33+0900
last_modified_at: 2023-01-17 21:48:34+0900
---

## 👻 애셋 확인하기
에픽 게임즈의 마켓에서 FPS와 적절한 애셋 하나를 다운받아 프로젝트가 추가시킨 후, 프로젝트를 열면 다운받은 애셋을 확인할 수 있다. 메쉬를 더블클릭하여 열거나 뷰포트로 드래그 앤 드롭하면 쉽게 확인이 가능하다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/create-character/skeletal-mesh.PNG)   

***

## 👻 캐릭터 생성하기
2D 게임을 만들 때와 비슷하게 블루프린트 클래스를 만들어 방금 다운 받은 애셋을 적용시키면 쉽게 고퀄리티의 캐릭터를 만들 수 있다. ``` Character ```를 상속받은 블루프린트 클래스를 하나 생성하자.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/create-character/bpclass-character.PNG)   

``` Character ```의 부모 클래스는 ``` Pawn ```이고, 유저가 플레이 가능한 클래스이다. 폰 클래스와 다른 점은 플레이어처럼 걷거나 수영, 점프 등의 **움직임 이벤트 적용**이 가능하다.

***

### 🌱 Mesh 적용하기
이제 생성한 블루프린트 클래스에 방금 다운받은 메쉬를 적용해보자. 해당 블루프린트 클래스의 상세 페이지로 들어가서 확인해보면 기본 구조는 다음과 같다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/create-character/mesh-component.PNG)   

컴포넌트 우측에 _Edit in C++_ 라고 적혀있는 컴포넌트들은 기본적으로 **삭제가 불가능**하다. 여기서 ``` Mesh ```를 눌러 우측 디테일창을 확인해보면 적용 가능한 ``` Skeletal Mesh ``` 가 있다. 여기에 적용시켜주면 간단하게 생성할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/create-character/mesh.PNG)   

> 💡 **Skeletal Mesh**   
인간의 신체 구조처럼 이루어져있는 메쉬이다. 수많은 폴리곤으로 이루어져 있으며 인간과 비슷하기 때문에 애니메이션을 보다 자연스럽게 만들 수 있다.

***

## 👻 캐릭터 이동하기
입력값의 관리를 편하게 하기 위해 ``` Edit 👉 Project Settings 👉Engine 👉 Input ``` 부분에 매핑할 입력값을 추가해준다. 앞뒤로 움직이는 ``` MoveForward ``` 이벤트에 ``` W ```와 ``` S ```를, 양옆으로 움직이는 ``` MoveRight ``` 이벤트에 ``` A ```와 ``` D ```를 각 스케일에 맞게 매핑해주었다.

그런 다음, 블루프린트 클래스로 돌아와 이벤트 그래프에 해당 이벤트들을 추가해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/create-character/add-movement-input.PNG)   

``` Set Actor Location ```을 사용해도 되지만 해당 함수는 액터의 위치를 절대값으로 받는다. 그렇게 되면 시간과 속력을 곱해줘서 이동 거리를 구한 다음 다시 현재 거리에서 이동 거리만큼 더하여 세팅해줘야하는 단점이 있다.

``` Add Movement Input ``` 함수는 현재 캐릭터가 나아가는 방향 벡터를 받고 추가로 스케일만 더 받으면 알아서 계산해주어 위치를 세팅해주게된다. 입력값을 매핑할 때 지정해주었던 스케일을 곧바로 반환받아 해당 함수에 넣어주면 따로 계산 없이 이동 이벤트를 만들 수 있다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/create-character/move-character.gif)   

***

## 👻 글을 마치며
이번 시간에는 3D 캐릭터를 생성하고 이동하는 방법에 대해 알아보았다. 생각보다 간단해서 놀랐고 애셋을 다운받아 적용하여 실습을 진행하니 이론만 공부했을 때보다 훨씬 더 재미있고 집중이 잘 됐던 것 같다. 다른 여러 기능을 테스트해보면서 에디터 사용법을 얼른 익혀야 할 것 같다.

***

_출처_
_[인프런 Rookies님 강의](https://inf.run/AXLS)_