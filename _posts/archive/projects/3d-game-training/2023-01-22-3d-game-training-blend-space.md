---
title: "[3D Game Training] #5. 캐릭터 스텝 변경하기"
excerpt: "블렌드 스페이스를 이용하여 더욱 자연스럽게 애니메이션 블렌딩해보기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, animation, blueprint, unreal, blend space]

permalink: /3d-game-training/blend-space/

toc: true
toc_sticky: true

date: 2023-01-22 17:50:28+0900
last_modified_at: 2023-01-22 17:50:32+0900
---

## 👻 스텝 변경하기
상하좌우키를 누르면 백 스텝, 사이드 스텝 등으로 변경해보자. **블렌드 스페이스(Blend Space)**를 이용하면 각도에 따라 원하는 애니메이션이 나타나도록 설정할 수 있다. 각도는 **플레이어가 걸어가고 있는 회전값**에서 마우스 커서에 해당하는 **에임(Aim) 회전값**을 뺀 값이다.

- **걸어가고 있는 방향**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/velocity-rotation.PNG)   

![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/velocity-rotation-result.gif)   

- **에임 방향**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/aim-rotation.PNG)   

![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/aim-rotation-result.gif)   

이제 두 회전값의 차이를 구해 확인하면 다음과 같다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/rotations-result.gif)   

***

## 👻 블렌드 스페이스
우클릭 ``` Animation 👉 Blend Space 혹은 Blend Space 1D ```를 이용해 블렌드 스페이스를 생성할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/create-blend-space.PNG)   

하나의 애니메이션만 블렌딩 할 것이라면 ``` 1D ```가 붙은 것을 사용하면된다. 나 역시 스텝 애니메이션만 변경하면 되기 때문에 ``` 1D ```를 생성해주었다.

***

### 🌱 세팅하기
블렌드 스페이스 상세 페이지로 들어가면 좌측에 수많은 세팅이 보이는데, 그 중 ``` Axis Settings ```에서 기준이 될 ``` Axis Value ```의 이름과 최소, 최대 각을 지정해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/axis-settings.PNG)   

그런 다음, ``` Asset Browser ```에서 해당 각마다 나타낼 애니메이션을 드래그 앤 드롭으로 설정해주고 좌측 ``` Blend Samples ```에서 값을 깔끔하게 조정해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/blend-settings.PNG)   

> 화면 정중앙에 최소, 최대의 각도를 가진 축이 나오는데, 여기에 애니메이션을 드래그 앤 드롭하면 각도마다 원하는 애니메이션을 세팅해줄 수 있다.   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/display.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/blend-result.gif)   

> 💡 **Ctrl**을 누른 상태로 마우스 커서를 옮기면 각 각도에 따른 애니메이션을 확인할 수 있다.

***

### 🌱 마무리
마무리로 **애니메이션 블루프린트(Animation Blueprint)**로 넘어와서 기존의 코드를 조금 수정해주면 완성이다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/set-movement-offset-yaw.PNG)   

``` MovementOffsetYaw ``` 변수를 하나 추가해주고 회전 변화량을 세팅해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/move-state.PNG)   

마지막으로 움직이는 상태를 담당하는 ``` Move(State) ```를 위와 같이 변경해주면 끝이다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/blend-space/blend-result2.gif)   

***

## 👻 글을 마치며
이번 시간에는 스텝을 각 방향에 맞게 변경해보았다. 아직까지는 여러개의 애니메이션을 동시에 섞어 쓸 때 어려운 부분이 많은 것 같다. 기본 게임 수학 쪽을 조금 더 깊게 공부해야 그나마 설계 부분에서 쉬워질 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/AXLS)_