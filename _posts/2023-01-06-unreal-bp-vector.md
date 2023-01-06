---
title: "[Unreal Engine] BP - 벡터(Vector)"
excerpt: "블루프린트에서의 벡터에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, vector, rotator]

permalink: /unreal/bp-vector/

toc: true
toc_sticky: true

date: 2023-01-06 19:31:06+0900
last_modified_at: 2023-01-06 19:31:09+0900
---

## 👻 벡터
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/vector1.jpg)   

**벡터(Vector)**는 **방향과 크기**를 합한 개념을 의미한다. 숫자로 크기를 나타내며 화살표로 방향을 나타낸다. 문자 위에 화살표를 적음으로써 나타낼 수 있다.

***

### 🌱 단위 벡터
크기가 1인 벡터를 **단위 벡터**라고 하며 x, y의 값을 각각 a, b라고 할 때 각 수를 크기로 나눈 벡터를 의미한다. 단위 벡터를 만드는 것을 **정규화(Normalization)**라고 한다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/vector2.jpg)   

***

### 🌱 벡터의 덧셈과 뺄셈
벡터끼리는 더하거나 뺄 수 있다. 각각의 좌표 값을 더하거나 빼면 쉽게 구할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/vector3.jpg)   

***

### 🌱 위치 벡터
위에서 본 벡터와 다른 의미로 사용되어지는 **위치 벡터**는 말 그대로 위치를 나타낸다. x, y 혹은 x, y, z 좌표를 이용하여 위치를 의미하는 좌표값을 가진다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/vector4.jpg)   

***

## 👻 로컬 좌표와 월드 좌표
언리얼 엔진 에디터에서 로컬 좌표와 월드 좌표를 각각 구분지어 확인할 수 있다. 화면 우측 상단의 해당 아이콘을 통해 각각의 기준이 되는 좌표를 변환할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/local-world.PNG)   

> 변환 단축키 : ```Ctrl + ` ```

월드 좌표는 전체 월드의 기준이 되는 **고정된 값**의 좌표이며 x, y, z의 방향이 정해져있다. 반면에 로컬 좌표는 상대적인 좌표이며 한 물체의 로컬 좌표는 해당 물체의 기준점을 통해 좌표값이 정해진다. 절대적인 값이 아니다.

각 객체마다 좌표값을 월드 좌표, 로컬 좌표로 변환할 수도 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/location.PNG)   

***

## 👻 실습
언리얼 엔진 에디터를 이용해 벡터를 실습해보자. 임의로 플레이어와 몬스터 객체를 만든 다음 플레이어가 몬스터의 방향으로 이동하도록 만든 코드이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/player-monster.PNG)   

길쭉하게 생긴 게 플레이어, 동그란 구가 몬스터이다. ☺

- **벡터 타입의 변수 추가하기**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/variable.PNG)   

각각 벡터, 단위 벡터를 의미한다.

- **해당 액터의 좌표 가져오기**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/get-actor-location.PNG)   

``` Get Actor Location ``` 노드로 해당 객체의 **위치 벡터**를 가져올 수 있다.

- **두 객체 간의 벡터 구하기**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/get-direction.PNG)   

플레이어가 몬스터 쪽으로 이동해야 하기 때문에 몬스터의 좌표값을 기준으로 삼은 벡터값을 구해야한다. 따라서 몬스터의 위치 벡터에서 플레이어의 위치 벡터를 빼면 방향과 크기를 가지는 벡터값을 구할 수 있다.

또, ``` Normalize ``` 노드를 이용하여 해당 벡터를 정규화 시키면 **단위 벡터**를 구할 수 있고, 그 값을 ``` NormalizedDirection ``` 변수에 세팅해주었다.

- **플레이어 객체 이동시키기**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/event-tick.PNG)   

이제 플레이어를 이동시키기 위해 **매 프레임마다 이벤트를 발생시키는** ``` Event Tick ``` 노드를 이용하여 블루프린트를 작성해주었다. 플레이어의 위치 벡터를 가져온 다음 단위 벡터를 이용하여 좌표값을 설정해주면 플레이어가 이동할 것이다.

> 💡 그냥 실행시키면 플레이어가 이동하지 않고 에러가 발생한다.   
>
> ![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/movable-error.PNG)   
>
> 해당 객체는 움직이지 않는다고 세팅되어 있기 때문에 발생하는 문제이다. **Mobility**를 ``` Movable ```로 변경해주면 에러를 고칠 수 있다.
>
> ![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/movable.PNG)   

- **매 초마다 일정 속력으로 이동시키기**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/event-tick-speed.PNG)   

위처럼 단위 벡터에 직접적인 수를 이용하여 계산을 하게 되면 각 컴퓨터의 성능마다 게임 속도의 차이가 발생하게 된다. 그래서 유동적으로, 임의의 속도를 지정하여 매 프레임마다 변화한 시간을 곱한 값을 단위 벡터와 연산하도록 수정해주었다. 이렇게 된다면 컴퓨터의 성능에 상관없이 정해진 속도로 각 비율에 맞게 이동할 것이다.

- **몬스터의 좌표값을 이용하여 플레이어 객체 이동시키기**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-vector/tick2.PNG)   

이제 속도를 유동적으로 계산하여 플레이어 객체를 이동시킬 수 있게 되었다. 이제 몬스터의 위치에 따라 알맞은 비율로 플레이어가 이동하도록 만들어주어야한다. 앞서 세팅해 준 플레이어와 몬스터 간의 벡터, 단위 벡터 값을 이용하여 계산 및 이동하도록 변경해주었다.

또한, 몬스터의 일정 사거리 내에 플레이어가 접어들게되면 이동을 멈추는 이벤트까지 추가해주었다.

> - **Event Tick : Delta Seconds**
    - 매 프레임마다의 시간 변화량
- **Vector Length**
    - 두 좌표 간의 거리
- **Set Actor Location**
    - 액터의 좌표값 세팅

***

## 👻 글을 마치며
이번 시간에는 벡터 이론 공부부터 실습을 통한 코드 구현까지 해보았다. 이제 좌표값을 알게 되었으니 액터의 이동까지 구현하고 원리를 이해할 수 있게 되었다. 코드가 복잡해지는 만큼 점점 더 재미있어 지는 것 같다. 공부하면서 동시에 내 머릿속에 있는 것들을 어떤 식으로 만들어야할지 많은 생각이 드는 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/vector-rotator/BP_Vector)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   