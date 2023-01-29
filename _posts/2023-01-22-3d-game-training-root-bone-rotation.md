---
title: "[3D Game Training] #6. 루트 본 회전 시키기"
excerpt: "일정 각도를 기준으로 루트 본, 애니메이션 커브를 사용하여 캐릭터 회전 시키기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, animation, blueprint, unreal, root bone, rotation, curve, metadata]

permalink: /3d-game-training/root-bone-rotation/

toc: true
toc_sticky: true

date: 2023-01-22 21:38:05+0900
last_modified_at: 2023-01-22 21:38:07+0900
---

## 👻 루트 본 회전시키기
**루트 본(Root Bone)**은 말 그대로 스켈레톤 메쉬에서 루트에 해당하는 뼈대를 의미한다. 마우스 커서를 움직여도 캐릭터 전체가 이동하지 않고 일정 각도(90도)까지는 움직이지 않다가, 기준 각도를 넘어서면 캐릭터를 회전시켜볼 것이다.

***

### 🌱 변수 추가하기
현재 플레이어가 바라보는 방향에 대한 정보와 변화했을 때의 정보를 가지고 계산을 해야하기 때문에 변수를 추가해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/add-yaw-variables.PNG)   

- ``` CharacterYaw ``` : 현재 캐릭터의 Yaw 값
- ``` PrevCharacterYaw ``` : 이전 프레임에서 캐릭터의 Yaw 값
- ``` RootYawOffset ``` : 루트 본의 Yaw 값

***

### 🌱 값 세팅하기
위에서 만들어 준 변수들의 값을 세팅하기 위해 ``` UpdateTurn ```이라는 함수를 만들어주고, 현재 플레이어의 Yaw 값을 구해 세팅해 주었다. 브랜치의 조건은 현재 플레이어의 속력(Speed)가 0일 때를 기준으로 삼았다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/update-turn.PNG)   

> 💡 ``` Normalize Axis ``` : 유효한 각도값을 체크해준다.

위에서 만든 값들을 프린트하여 확인해보면 다음과 같이 잘 구해진 것을 확인할 수 있다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/check-rotation.gif)   

***

### 🌱 애니메이션 추가하기
왼쪽이나 오른쪽으로 90도 이상 캐릭터가 돌아갔을 때 루트 본을 회전시키는 애니메이션을 추가해주자. 대기 상태에서만 유효한 조건이므로 ``` Idle(State) ```을 먼저 바꿔주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/idle.PNG)   

스켈레톤의 상하체를 **허리(Spine_01)** 기준으로 블렌딩 해주었고 상체는 ``` Idle Pose ```로, 하체는 아래와 같이 세팅해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/idle-lower-body.PNG)   

> 💡 각각의 애니메이션에서 ``` Loop Animation ``` 체크를 해제해주어야 **무한 반복**되지 않는다. 회전 시 나타낼 애니메이션은 한 번만 하면 되기 때문에 체크 해제해 주었다.   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/loop-animation.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/rotate-root-bone-result.gif)   

왼쪽, 혹은 오른쪽으로 돌아야 하는 상황일 때 불리언 값을 설정해주면 애니메이션 적용이 되고, 마지막으로 ``` AnimGraph ```를 수정해주면 완성이다.

- **Zero to Left**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/zero-to-left.PNG)   

- **Left to Zero**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/left-to-zero.PNG)   

- **Zero to Right**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/zero-to-right.PNG)   

- **Right to Zero**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/right-to-zero.PNG)   

- **AnimGraph 수정**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/rotate-root-bone.PNG)   

> 💡 ``` Rotate Root Bone ```   
``` Yaw ``` 값 외엔 필요없는 핀들은 우측 디테일 창에서 연결을 해제시켜주면 숨길 수 있다.   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/bone-setting.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/rotate-root-bone-result2.gif)   

***

## 👻 애니메이션 커브
이제 몸을 돌리는 듯한 애니메이션에 진짜로 캐릭터를 돌려주는 기능을 붙여보자. **애니메이션 커브(Animation Curve)** 기능을 사용하면 해당 애니메이션에 커브값을 가지고 방향을 계산할 수 있다. 왼쪽, 오른쪽 90도로 돌아가는 애니메이션으로 들어가 **커브(Curve)**를 추가해주자.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/create-curve.PNG)   

``` Create Curve ```를 눌러 새 커브를 만들어주고 값을 정해주자. 왼쪽으로 돌아가는 애니메이션은 **90도에서 0도**로, 오른쪽으로 돌아가는 애니메이션은 **-90도에서 0도**로 변화하는 값을 가진다. 각 지점마다 값은 **키(Key)**를 추가하여 설정해줄 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/add-key.PNG)   

0초, 중간초, 마지막초에 키를 추가하고 각도의 변화량을 입력해주었다. 그 다음, 키를 모두 선택하고 좌측 상단의 ``` Zoom to Fit ```을 클릭해주면 한 눈에 볼 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/zoom-to-fit.PNG)   

그래프를 자연스럽게 연결해주려면 ``` 우클릭 👉 Auto ```를 누르면 자동 보정된다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/auto.PNG)   

이제 다시 애니메이션 전체 관리 창으로 돌아와 커브에 **메타데이터(Metadata)**를 추가해주자. 해당 값으로 현재 커브가 진행중인지 아닌지를 확인할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/add-metadata.PNG)   

난 ``` Turning ```이라는 이름으로 지어주었고, 현재 애니메이션 커브가 활성화면 1, 비활성화면 0의 값을 가진다.

***

### 🌱 적용하기
이제 다시 **애니메이션 블루프린트**로 돌아와 함수를 하나 추가해주자. ``` Update Turn Animation ``` 함수가 실행될 때마다 캐릭터를 90도 회전할 것이다. ``` Event Graph ```에 연결을 추가해주는 것도 잊지 말아야한다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/update-turn-animation.PNG)   

우선 브랜치를 하나 만들어 현재 커브의 메타데이터 값이 0보다 큰지 확인한다. True면 커브가 진행중이라는 의미이다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/update-turn-animation2.PNG)   

커브가 진행중이라면 캐릭터의 루트 본을 회전시켜 줄 것이다. 커브 또한 이전 프레임의 커브값과 현재의 커브값 둘 다 사용하여 계산할 것이기 때문에 변수를 새로 만들어 세팅해준 후 계산을 진행하였다.

> **루트 본 = 커브의 변화량 + 현재 루트 본**

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/curve-result.gif)   

현재 시퀀스 1에만 위의 코드를 적용시켜 두었는데 결과를 보면 캐릭터가 성공적으로 회전하긴 하지만 이상한 방향으로 돌아가는 것을 확인할 수 있다. 자세히보면 왼쪽, 또는 오른쪽 방향으로 90도가 **더 진행된 상태에서 시작**하여 원래 자리로 회전한다는 것을 알 수 있는데, 이는 기존의 루트 본 값이 회전 방향의 반대 방향으로 감소되어 있는 상태이기 때문이다. 그래서 처음 오른쪽으로 돌리게 되면 **루트 본은 -90도의 값을 가지고 있는 상태에서 -90도를 또 더하니 -180도의 위치에서 시작**하고, 왼쪽은 반대로 **90 + 90 = 180도**의 위치에서 회전을 시작하게 되는 것이다.

이러한 결과를 수정하기 위해 ``` Clamp ```를 이용하여 **루트 본의 값을 -90도에서 90도 사이로 제한**하였다. 그렇게 되면 -180도의 값이 되어도 **-90도**를 반환하게 되고 그 뒤로는 정상적인 계산이 진행되는 것이다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/sequence2.PNG)   

시퀀스 2에 위의 한 단계를 더 추가시켜주면 정상적으로 캐릭터가 회전하게 된다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/root-bone-rotation/curve-result2.gif)   

***

## 👻 글을 마치며
이번 시간에는 루트 본을 회전하는 방법과 더 나아가 애니메이션 커브를 이용해 회전을 자연스럽게 하는 방법에 대해 알아보았다. 확실히 계산할 게 많아지니 너무 복잡하지만 😭 그래도 재미있어서 포기하지 않고 천천히라도 앞으로 나아갈 수 있는 것 같다. ~~근데 다시 혼자 만들어보라고 하면 못 할 것 같다..~~ 이제 강의를 보지 않고 혼자 만들 수 있도록 개인 프로젝트를 하나 정해서 복습을 시작해야겠다. ~~그 전에 아이디어를 먼저 정해야하는데..이게 제일 어려운 것 같다. 😂😂😂~~

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/AXLS)_