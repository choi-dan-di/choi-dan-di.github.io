---
title: "[Paper2D Training] #3. 캐릭터 애니메이션 갱신하기"
excerpt: "입력키 변화가 있을 때의 캐릭터 애니메이션 갱신해보기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, animation, update animation]

permalink: /paper2d-training/update-animation/

toc: true
toc_sticky: true

date: 2023-01-07 18:56:08+0900
last_modified_at: 2023-01-07 18:56:05+0900
---

## 👻 필요한 정보 찾기
이번 시간에는 방향키 입력에 따라 움직이는 플립북을 세팅해 볼 것이다. 우선 그러려면 내가 현재 **키를 누르고 있는지, 누르고 있다면 어느 방향인지**에 대한 정보가 필요하다.

***

### 🌱 방향 정보 저장
방향에 대한 정보를 저장하기 위해 enum 파일을 추가해주고 상하좌우 정보를 세팅해 주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/update-animation/edirection.PNG)   

그리고 해당 enum을 타입으로 가지는 ``` Direction ``` 변수를 추가해주었고, 각 키를 입력하면 해당 변수에 값을 세팅해주도록 하였다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/update-animation/set-direction.PNG)   

> 뒤쪽의 플립북을 세팅하는 코드는 ``` Update Animation ``` 함수로 빼주었다.

***

### 🌱 키의 누름 유무
그 다음, 키가 현재 눌려져있는 상태인지 아닌지의 정보를 저장하기 위해 불리언 변수 ``` KeyboardPressd ``` 변수를 추가해주고 매 프레임마다 실행되는 **Event Tick**을 이용해 값을 세팅해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/update-animation/event-tick.PNG)   

키가 눌려져 있으면 True, 눌려져 있지 않으면 False 값을 가진다.

***

## 👻 애니메이션 갱신하기
이제 애니메이션을 갱신하는 데에 필요한 모든 데이터를 알 수 있으니 분기문을 이용해 설정만 해주면 완성된다. 플립북 세팅 부분을 따로 빼둔 ``` Update Animation ``` 함수에 분기문을 넣어 알맞은 플립북을 세팅하도록 만들어주었다. 키가 눌려져 있으면 ``` _move ```를, 눌려져 있지 않으면 ``` _idle ```를 세팅하도록 해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/update-animation/update-animation.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/update-animation/move.gif)   

애니메이션을 성공적으로 갱신할 수 있었다.

하지만 이렇게 하니 키를 눌렀다 뗐을 때 멈추지 않는 버그가 발생했다. 이 버그를 해결하기 위해 키를 입력하지 않을 때에도 ``` Update Animation ``` 함수 호출을 해주었다. 그러면 방향은 유지된 채로 ``` KeyboardPressed ``` 값은 **False**로 다시 ``` _idle ``` 상태가 되기 때문이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/update-animation/equal.PNG)   

***

## 👻 공격하기
이번엔 스페이스바를 누르면 공격 애니메이션이 나오도록 만들어보자. 이전 시간에 알아보았던 입력 매핑을 이용해 스페이스 바 이벤트를 **Attack**이라는 이름으로 매핑시켜주었다. 이번에는 **Axis Mappings**가 아닌 **Action Mappings**에 추가해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/update-animation/action-mappings.PNG)   

매핑된 이벤트를 적용하는 법은 이전과 같다. 공격 중인지 판단하는 불리언 변수 ``` Attack ```을 추가해 준 다음 해당 값을 변경시켜주며 공격 유무를 저장한다. 그런 다음 한 플립북 모션이 걸리는 시간을 반환해주는 ``` Get Flipbook Length ``` 노드를 이용해 소요 시간을 구해준 뒤, 해당 시간만큼 ``` Delay ``` 해주고 Attack 변수를 다시 False 값으로 변경시켜주면 공격 애니메이션이 반복되지 않고 한 번만 실행된다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/update-animation/input-attack.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/update-animation/attack.gif)   

***

## 👻 글을 마치며
이번 시간에는 캐릭터 애니메이션을 갱신해보았다. 키 입력 하나에도 많은 코드가 짜여진다는 것을 알게 되었고 확실히 알고리즘의 중요성이 점점 커지는 것 같다. 이 조그마한 게임 하나 만드는 데에도 많은 코드가 들어간다고 생각하니 재미있을 것 같으면서도 막막할 것 같은 느낌이 든다. 그래도 공부를 확실히 해두면 벽에 부딪혀도 금방 일어서서 나아갈 수 있지 않을까 하고 생각한다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/ji8q)_