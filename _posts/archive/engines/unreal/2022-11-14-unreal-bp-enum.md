---
title: "[Unreal Engine] BP - Enum"
excerpt: "블루프린트에서 열거형인 Enum에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, enum]

permalink: /unreal/bp-enum/

toc: true
toc_sticky: true

date: 2022-11-14 21:32:23+0900
last_modified_at: 2022-11-14 21:32:26+0900
---

## 👻 열거형을 사용하는 이유
**관련된 상수를 한데 묶어 놓은 방법**을 **열거형(enum)**이라 한다. 

사용되는 예를 들어보자면, RPG 게임을 만들 때 플레이어가 가만히 있을 수도 있고, 걸어다닐 수도 있고, 공격할 수도 있다. 이러한 상태마다 관련된 애니메이션들이 있을텐데, 알맞은 상태에 알맞은 애니메이션을 출력하려면 **상태에 관한 정보들을 저장**해두어야 한다. 

보통 변수를 하나 지정해두고 각 상태마다 값을 부여해 저장하는데, 만약 열거형을 사용하지 않는다면 상태와 관련된 변수들이 많아질수록 하드코딩하는 수가 늘어나게되고 의미가 헷갈려서 실수할 일이 잦아질 수 있다. 이러한 일들을 방지하기 위해선 열거형으로 상태를 저장하는 방식이 **상태 정보를 관리하기에 알맞은 방식**이라고 할 수 있다.

***

## 👻 Blueprint Enum File
언리얼 엔진 에디터에서는 Enum 파일 형식이 따로 존재한다. 해당 파일을 만들어서 블루프린트의 Enum 상수를 관리할 수 있다. 파일을 생성해 상수를 설정해서 사용해보자.

***

### 🌱 Enum 파일 생성하기
콘텐트 브라우저에서, ``` Content ``` 안에 ``` Blueprints ```라는 폴더를 하나 만들고   
``` 우클릭 👉 Blueprints 👉 Enumeration ```을 클릭해서 Enum 파일을 하나 생성해주자. 이름은 **대문자 E**를 붙이고 만드는 방식이 일반적이다. 난 상태를 나타내는 상수들을 만들기 위해 **EState**로 지어주었다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-enum/create-enum.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-enum/enum-file.PNG)   

파일이 만들어지면 해당 파일을 더블클릭해 상수 설정 페이지로 이동해주자.

***

### 🌱 Enum 설정하기
페이지로 넘어왔으면 ``` Add Enumerator ```를 클릭하여 세 가지 상태를 지정해주자.

> 각각의 상수는 해당값을 나타낸다.   
- **Idle** : 0 - 아무것도 안 함
- **Moving** : 1 - 움직임
- **Attack** : 2 - 공격함

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-enum/estate.PNG)   

잊지않고 저장 해주는 습관을 들이도록 하자.

***

### 🌱 Enum 상수 사용하기
다시 블루프린트 화면으로 넘어와서 변수를 하나 생성해 준 후에 타입에서 **EState**를 검색해주면 Enum 타입으로 뜨게된다. 컴파일, 저장 후 우측에서 Default Value를 살펴보면 **State**값이 우리가 이전에서 설정한 세 가지로 뜨는 것을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-enum/state.PNG)   

이렇게 되면 0, 1, 2로 값을 설정했던 것보다 가독성도 좋아지고 실수할 일이 적어지게된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-enum/get-set.PNG)   

변수와 똑같이 Get, Set도 가능하다.

> 💡 **열거형 비교**   
열거형의 변수 값을 비교하고 싶으면 **Enum** 타입의 비교 연산 노드를 호출하여 사용하면 된다.   
(단, Equal, Not Equal만 가능하며 대소 비교는 불가능하다.)   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-enum/equal-enum.PNG)  

***

#### 🪐 Switch on
``` Switch on ~ ``` 노드를 사용하면 ``` C++ ```의 ``` switch문 ```처럼 사용이 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-enum/switch.PNG)   

***

## 👻 열거형의 타입
열거형도 결국은 0, 1, 2, ...의 값을 가지기 때문에 정수와 비슷하다고 생각하면 된다. (색도 비슷하다.) 단지 관련된 고정값들을 편하게 정리하고 관리하기 쉽게 만든다는 것에 의의를 둔다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-enum/to-string.PNG)   

위의 블루프린트를 실행시키면 ``` Idle ```이라는 결과값이 나오게된다. 해당 블루프린트는 **변수 State의 값을 Byte로 변환한 후, string 형식으로 캐스팅, 그리고 그 캐스팅된 값을 출력하라**는 의미이다.

***

## 👻 글을 마치며
이번 시간에는 블루프린트에서 어떻게 열거형을 정의하고 사용하는지에 대해 알아보았다. 파일을 따로 관리할 수 있어서 굉장히 좋은 것 같다. 그리고 게임을 개발한다고 가정했을 때 얼마나 많은 상태값을 한 변수에 넣을지도 사실상 상상이 잘 가지 않는다. 그래도 익숙해지면 하드코딩 하던 때보다 훨씬 편리해질 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/flow-control/BP_Enum)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   