---
title: "[3D Game Training] #3. 애니메이션 적용시키기"
excerpt: "3D 캐릭터에 애니메이션을 적용시켜보기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, rotation, character, animation]

permalink: /3d-game-training/character-animation/

toc: true
toc_sticky: true

date: 2023-01-19 20:43:08+0900
last_modified_at: 2023-01-19 20:43:11+0900
---

## 👻 애니메이션 적용시키기
지금은 캐릭터가 움직일 때 서 있는 상태로 위치만 바뀐다. 이제 앞으로 가는 애니메이션을 캐릭터에 적용시켜서 해당 입력값이 들어오면 애니메이션이 출력되도록 만들어보자. 애니메이션은 애셋을 다운 받을 때 같이 다운 받았던 것들을 사용하였다.

***

### 🌱 애셋 애니메이션
애니메이션을 적용시킬 플레이어 클래스의 ``` Animation ``` 부분을 수정하면 바로 적용시킬 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/set-animation-mode.PNG)   

아까 애셋 애니메이션을 사용해보기로 하였으니 ``` Use Animation Asset ```을 선택한 후 전진하는 애니메이션을 설정해보자.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/set-animation-mode2.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/character-animation.gif)   

적용은 잘 되었지만 움직이지 않는 상태에도 전진 애니메이션이 적용되어있다는 것을 알 수 있다.

***

### 🌱 애니메이션 블루프린트
내가 원하는대로 애니메이션을 관리하고 싶다면 **애니메이션 블루프린트(Animation Blueprint)**를 만들어 관리하는 방법이 있다. 우클릭한 후 애니메이션 탭에서 찾아 생성하면 된다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/animation-blueprint.PNG)   

그런 다음 ``` Anim Graph ```에서 전진 애니메이션을 연결시켜주면 아까전과 같은 동작을 하는 코드가 완성이 된다. 물론 ``` Use Animation Asset ```을 ``` Use Animation Blueprint ```로 바꿔주어야 한다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/set-animation-bp.PNG)   

애니메이션 블루프린트의 ``` Anim Graph ```는 다음과 같이 작성하였다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/anim-graph.PNG)   

> 애셋 애니메이션은 우측 하단 ``` Asset Browser ```에서 찾을 수 있다.   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/asset-browser.PNG)   

***

### 🌱 기능별 애니메이션
이제 전진하면 전진 애니메이션을, 멈추면 대기 애니메이션을 적용시켜 볼 것이다. 위에서 연결했던 ``` Jog_Fwd ``` 애니메이션의 연결을 삭제해주고, ``` 우클릭 👉 Add New State Machine ```을 통해 상태에 따라 애니메이션을 관리하는 노드를 만들어주자. 더블 클릭하여 들어가면 상태의 흐름을 나타내는 코드를 작성할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/choose-anim.PNG)   

처음엔 멈춰있는 상태였다가 키를 입력하면 움직이는 상태로 바뀌기 때문에 위와 같이 화살표를 연결해주었다. 양방향 화살표가 그려진 노드를 더블 클릭하면 상태를 변경하려는 조건을 입력해줄 수 있는데 ``` Boolean ``` 값만을 받아 True면 상태를 변경시키고, False면 상태를 변경시키지 않는 흐름도를 작성해 줄 수 있다.

그러기 위해서는 매 프레임마다 체크를 해주어서 ``` Boolean ``` 값을 세팅해 주어야하는데, 그 부분은 ``` Event Graph ```에서 할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/event-bp.PNG)   

``` Event Tick ```과 비슷하지만 애니메이션을 관리한다는 차이가 있다. 위의 노드를 이용하여 ``` Moving ```이라는 움직임 상태를 가지는 변수를 하나 생성해주고, 해당 변수를 아까 상태 흐름도에 이어붙이면 완성된다. 현재 플레이어의 속도(Vector)값을 받아와 0보다 크면 움직이는 것으로 간주한다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/idle-to-move-bp.PNG)   

대기 상태에서 움직이는 상태로 변경 시에 실행되는 코드이다. 반대로 작성시엔 ``` NOT Boolean ``` 노드를 추가해주면 된다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/character-animation/animation-final.gif)   

***

## 👻 글을 마치며
이번 시간에는 애셋 애니메이션을 어떻게 적용하는지, 또 애니메이션 블루프린트로 어떻게 애니메이션을 관리하고 기능을 수정하는지 알아보았다. 확실히 눈으로 보이는 게 많아 재미있지만 워낙 애니메이션 관련 파일 종류가 다양해 자세히 공부할 필요가 있는 것 같다. 지금은 크게 훑어보는 시야를 가지는 게 중요할 것 같고 블루프린트 작성에 크게 어려움을 겪지 않을 정도로 기량을 끌어올려두면 좋을 것 같다.

***

_출처_
_[인프런 Rookies님 강의](https://inf.run/AXLS)_