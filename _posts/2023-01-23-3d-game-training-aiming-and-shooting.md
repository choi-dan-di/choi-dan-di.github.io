---
title: "[3D Game Training] #10. 크로스헤어(CrossHair) 추가, 슈팅 구현하기"
excerpt: "충돌을 이용해 피격 판정 하는 방법에 대해 알아보고 크로스헤어 추가, 슈팅 구현해보기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, animation, blueprint, unreal, collision, ray casting, shooting, aiming]

permalink: /3d-game-training/aiming-and-shooting/

toc: true
toc_sticky: true

date: 2023-01-23 23:45:53+0900
last_modified_at: 2023-01-23 23:45:57+0900
---

## 👻 충돌 설정하기
크로스헤어를 설정하고 슈팅 기능을 만들어보자. **크로스헤어(Crosshair)**는 십자선 방식의 조준점을 의미한다. 그 전에, ``` Collision ```과 관련된 기능을 이용해 각 물체 간의 충돌 규칙을 설정해줄 수 있다. 우선, ``` Actor ```를 상속 받은 블루프린트 클래스 하나를 ``` BP_Box ```라는 이름으로 만들어주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/add-collision.PNG)   

``` Add ```를 눌러 콜리전을 검색하면 모양이 다른 충돌 캡슐을 추가할 수 있다. 테스트만 해 볼 것이기 때문에 **박스 콜리전(Box Collision)**을 추가해주었다.

우측 디테일 창의 ``` Collision ```을 살펴보면 다양한 충돌 규칙을 지정해 줄 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/collision-presets.PNG)   

``` Collision Presets ```는 기본으로 정해져 있지만 지금은 충돌 테스트를 해보기 위해 ``` Custom ```으로 변경하였다.

> - ``` Object Type ``` : 해당 오브젝트의 타입을 설정
- ``` Trace Responses ``` : 해당 오브젝트를 기준으로 어떤 행동에 설정하는 충돌 감지
- ``` Object Responses ``` : 상대 오브젝트 간의 충돌 감지

충돌의 허용 범위는 ``` Ignore ＜ Overlap ＜ Block ``` 순이며, 순서대로 **완전 무시, 겹침, 블로킹**의 의미이다.

플레이어의 경우 오브젝트 타입이 ``` Pawn ```으로 되어있기 때문에, ``` WorldDynamic ↔ Pawn ``` 중 둘 중(플레이어, 박스) 하나라도 ``` Ignore ```에 체크되어있다면 두 물체는 충돌이 성립되지 않는다. 반대로 둘 모두 ``` Block ```이 선택되어 있어야만 충돌이 성립된다.

> 💡 **충돌의 허용 범위가 좁을수록 실행 우선순위가 높아진다.**

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/collision-result.gif)   
<span style="font-size: 0.7rem; color: gray;">BP_Player, BP_Box에서 서로에 대한 충돌값이 모두 Block으로 설정되어있어야 충돌이 적용된다.</span>

***

### 🌱 Custom Collision
충돌은 입력키 매핑과 같이 ``` Project Settings 👉 Engine 👉 Collision ```에서 커스텀 할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/engine-collision.PNG)   

여기서 설정한 후 다시 블루프린트 클래스로 돌아오면 적용되는 것도 확인할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/engine-collision2.PNG)   

***

## 👻 충돌 확인하기
총 게임의 경우 원거리에서 총을 발사하여 해당 물체에 총알이 맞는지 안 맞는지 확인 후에 이벤트를 실행시켜야 할 것이다. 그럴려면 충돌을 하는지, 즉 **플레이어가 총을 쐈을 때 총구 기준 일직선 앞에 물체가 있는지**를 확인이 필요하다. ``` Line Trace ```와 관련된 노드를 이용하면 간단하게 충돌 유무를 판별할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/line-trace-by-channel.PNG)   

위 노드는 **트레이스 채널(Trace Channel)**을 기준으로 충돌 여부를 반환해주는 노드이다. 트레이스 채널은 위에서 잠깐 언급했듯 **어떤 행동에 설정**하는 충돌을 감지한다. 여기서는 **마우스 좌클릭**을 했을 때 ``` Start ``` 지점에서 ``` End ``` 지점까지 일직선의 **광선(Ray)**을 쏘아 그 사이에 어떠한 오브젝트가 있는지 ``` MyAttackRange ```라는 트레이스 채널에 의한 결과를 반환해준다.

> 💡 플레이어가 **마우스 좌클릭**(어떠한 행동)하여 공격했을 때, 해당 범위(Start to End)에 **충돌하는 물체**(충돌 대상 : ``` MyAttackRange ```의 값이 ``` Block ```으로 되어있는 오브젝트)가 있으면 ``` Hit Result Structure ``` 를 반환한다는 **흐름**을 잘 이해해야 한다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/line-trace-result.gif)   

> 이 밖에서 프로필을 이용하는 ``` Line Trace By Profile ```, 오브젝트를 이용하는 ``` Line Trace For Objects ``` 등 여러 라인 트레이스 노드가 존재한다.

***

## 👻 Ray Casting
위에서 **광선**을 이용해 확인하는 방법을 **레이 캐스팅(Ray Casting), 레이 트레이싱(Ray Tracing)**이라고 한다.

> 💡 **레이 캐스팅(Ray Casting)이란?**   
직역하면 **광선 투사**라는 뜻으로, 컴퓨터 그래픽스와 계산기하학의 다양한 문제를 해결하기 위해 **광선과 표면의 교차 검사를 사용하는 기법**을 말한다. 우리가 비록 3D 게임을 개발하고 있지만 모니터, 게임 화면 모두 2D라는 평면 구조에 속한다. 레이 캐스팅을 이용하면 2D 화면에서 3D 화면을 구현하는, 즉 원근감을 표시할 수 있게 해준다.

***

## 👻 조준점 만들기
아트 리소스를 하나 추가하고 **HUD(Head-Up Display)**를 이용하여 조준점을 만들어주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/draw-texture.PNG)   

화면의 정중앙보다 조금 더 위에 위치시키기 위해 X, Y의 좌표값을 2로 나누고 Y는 50을 더 빼주었다. 크로스헤어 이미지의 크기를 64x64로 지정했기 때문에 UV 좌표임을 고려하여 반인 32를 더 빼주었다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/crosshair.PNG)   

***

## 👻 슈팅하기
마우스를 좌클릭하면 크로스헤어가 가리키는 곳에 총알이 나가야한다. 우선 내가 보고있는 카메라의 위치와 크로스헤어가 가리키는 위치를 구하여 라인 트레이스를 해보자.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/camera-to-aim.PNG)   

- ``` Get Viewport Size ```
해당 노드를 이용하면 **가로, 세로 값**을 구할 수 있다. 반을 나누면 화면의 **정중앙**, Y 값에 50을 추가로 더 빼주면서 크로스헤어의 **2D 좌표값**을 구할 수 있었다.

- ``` Deproject Screen to World ```
해당 노드는 플레이어의 보기에 해당하는 카메라의 **2D 화면 공간 좌표를 3D 세계 공간 점과 방향으로 변환**해준다. 크로스헤어의 2D 좌표에 해당되는 3D 공간 좌표값을 반환해 줄 것이다.

이제 좌표값은 모두 구했으니 계산만 해주면 된다. 반환되는 ``` World Direction ```은 **방향 벡터**이므로 큰 값을 곱한 후 ``` World Location ```에 더해주었다. 여기에 ``` Line Trace ```를 사용하여 시각화를 하면 현재 내가 보는 위치(카메라)에서 일직선상으로 나타나게 될 것이다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/crosshair-result.gif)   

***

### 🌱 총구 찾기
성공적으로 크로스헤어가 가리키는 곳에 클릭이 된 것을 알 수 있다. 하지만 총알의 출발점이 플레이어의 카메라가 아닌 캐릭터가 들고있는 총의 **총구**가 되어야한다. ``` Socket ```을 사용하면 총구를 설정해줄 수 있다.

캐릭터의 스켈레탈 메쉬로 들어가 ``` weapon ``` 산하에 **소켓(Socket)**을 하나 추가해주고 위치를 총구로 조절해주면 완성이다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/add-socket.PNG)   

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/set-weapon-socket.PNG)   
<span style="font-size: 0.7rem; color: gray;">X축이 앞을 향하므로 잊지 말고 방향도 맞춰주자.</span>

플레이어 블루프린트 클래스로 넘어가 ``` Get Socket Transform ``` 노드를 사용하면 출발점을 총구로 쉽게 세팅할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/get-socket-transform.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/aiming-and-shooting/socket-result.gif)   

***

## 👻 글을 마치며
이번 시간엔 충돌 기초부터 조준점을 설정하고 슈팅하는 것까지 구현해보았다. 3D 게임을 개발할 땐 게임 수학만 이해하면 될 줄 알았는데 다양한 기법도 많고 심지어 그것들이 중요한 부분을 차지해서 전부 공부하느라 시간이 많이 걸린 것 같다. 레이 캐스팅이 되게 중요한 부분같아 시간이 날 때마다 자세히 찾아봐야 할 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/AXLS)_