---
title: "[3D Game Training] #2. 캐릭터 회전 & 점프하기"
excerpt: "캐릭터를 Z축 방향으로 회전시켜보고 점프 기능 추가하기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, rotation, player controller]

permalink: /3d-game-training/character-rotation/

toc: true
toc_sticky: true

date: 2023-01-17 23:36:14+0900
last_modified_at: 2023-01-18 00:05:21+0900
---

## 👻 캐릭터 회전하기
``` Q ```와 ``` E ```를 눌러 캐릭터를 Z축 기준으로 회전시켜보자. 우선 그러기 위해선 3차원 공간에서의 회전에 대해 알아야한다.

***

### 🌱 Roll, Pitch, Yaw
X, Y, Z 이 세개의 회전축에 대해 돌아가는 회전을 각각 **Roll, Pitch, Yaw**라고 한다. 각 액터의 기즈모(축과 회전축의 색이 같음)를 통해 알 수 있지만 더 쉽게 외우려면 **오른손 법칙**을 이용하면 된다.

> 💡 **오른손 법칙**   
기본적으로 회전 축 자체를 나타내는 방법도 있지만 자기장의 방향을 아는 법칙을 적용시키면 회전 방향을 쉽게 알 수 있다. **오른손을 엄지척(따봉) 모양으로 만들어서 회전축을 일치시키면 나머지 네개의 손가락이 향하는 방향이 회전 방향**이다.   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-rotation/right-hand.png)   

~~예시로 비행기가 유명하지만 개인적으로 나는 오른손 법칙이 더 편한 것 같다.~~

***

### 🌱 이벤트 추가하기
Z축에 해당하는 **Yaw** 방향으로 캐릭터를 회전시켜야한다. ``` WASD ```에 이벤트를 매핑했던 것처럼 ``` Q ```, ``` E ```도 동일하게 매핑해준다. 이벤트 이름은 ``` Turn ```으로 지어주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-rotation/turn-event.PNG)   

회전 방향에 따라 알맞은 스케일(1 혹은 -1)을 회전 속도(각도), 델타 세컨즈와 곱해주면 얼마큼 회전했는지 알 수 있고, 현재 회전값에 더해 세팅해주면 회전이 될 것이다. 물론 초기의 좌표 설정과 비슷하게 만들게되면 복잡하니 더 간단하게 만들어보자.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-rotation/add-actor-local-rotation.PNG)   

``` Add Actor Local Rotation ``` 함수를 사용하여 회전값의 변화량만 구해주면 쉽게 캐릭터의 회전이 가능하다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-rotation/rotation-character.gif)   

***

### 🌱 Add Controller
``` Add Controller ```로 시작하는 함수 중 회전 기능을 하는 함수가 추가로 존재한다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-rotation/add-controller.PNG)   

언뜻보면 위에서 만든 캐릭터 회전 이벤트와 동일하게 동작할 것 같지만 ``` Turn ``` 이벤트를 위의 함수들에 연결하여 사용하면 아무것도 동작하지 않는다.

이제까지 ``` BP_Player ```라는 블루프린트 클래스의 회전값을 건드리고 있었는데 ``` Add Controller ``` 함수는 말 그대로 컨트롤러의 회전값을 변경하는 함수이다. 블루프린트 클래스에 현재 컨트롤러가 따로 연결되어있지 않다. 그리고 게임을 실행시켜보면 ``` PlayerController ```가 자동으로 생기는 것을 확인할 수 있다. 바로 이 컨트롤러의 회전값이 건드려지게 되는 것이다. 이러한 문제를 해결하려면 ``` BP_Player ```**의 디테일에서 설정**을 변경해주거나 ``` Character Movement ```**의 설정**을 변경해줘야한다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-rotation/pawn-rotation.PNG)   

위의 설정은 **Yaw 회전을 컨트롤러의 것을 사용하겠냐**는 의미이다. 이렇게 하면 문제없이 ``` Add Controller ``` 함수를 이용하여 회전을 구현할 수 있다.

***

## 👻 마우스 방향으로 회전하기
키를 누르는 것이 아닌, 마우스가 움직이는 방향으로 회전시켜보자. **Yaw** 방향은 이전에 만들었던 것과 똑같이 하되 입력값을 ``` Mouse X ```로 변경해주면 되고, 위아래로 움직일 땐 플레이어는 가만히, 카메라만 이동시켜야한다.

``` SpringArm ```에도 마찬가지로 회전 설정이 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-rotation/camera-rotation.PNG)   

``` Use Pawn Control Rotation ```을 활성화 시켜주면 카메라만 움직이게 된다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-rotation/rotation-character-mouse.gif)   

***

## 👻 점프하기
마찬가지로 스페이스 바에 점프 이벤트를 매핑시키고 적용해보자.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-rotation/jump.PNG)   

이미 ``` Character ```에 점프 이벤트를 적용시켜주는 함수가 존재한다. 플레이어 클래스 내에서 사용하게되면 바로 ``` Jump ``` 함수를 사용하면 되지만 나 같은 경우는 중간에 ``` PlayerController ```로 모든 기능을 빼주었기 때문에 ``` Get Player Character ```라는 함수를 사용하여 플레이어를 가져온 다음 점프 이벤트를 붙여주었다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-rotation/jump-character.gif)   

여기서 ``` Character Movement ```를 조금 더 손 본다면 원하는대로 점프 속성을 수정해줄 수 있다.

***

## 👻 글을 마치며
이번 시간에는 캐릭터가 회전하고 점프하는 이벤트를 구현해보았다. 회전축이 내가 생각했던 방향과 달라 고민을 좀 많이 했었는데 그래도 결국에 알아냈고, 그 성취감이 너무 좋은 것 같다. 또한 언리얼이 많이 무겁긴 하지만 그만큼 기능이 많아 다양한 기능을 쉽게 적용할 수 있다는 것이 장점 중의 장점인 것 같다.

***

_출처_
_[인프런 Rookies님 강의](https://inf.run/AXLS)_