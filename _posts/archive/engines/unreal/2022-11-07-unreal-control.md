---
title: "[Unreal Engine] 액터(Actor) 다루기"
excerpt: "언리얼 엔진 에디터의 기본 조작 방법을 알아보고 액터를 컨트롤해보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, control]

permalink: /unreal/control/

toc: true
toc_sticky: true

date: 2022-11-07 19:09:06+0900
last_modified_at: 2022-11-07 19:09:08+0900
---

## 👻 액터(Actor) 컨트롤하기
이번 시간엔 뷰포트 상에 배치된 액터를 컨트롤하는 방법에 대해 알아보도록 하자.

***

### 🌱 뷰포트 카메라 조작하기
우선 언리얼 엔진을 실행시킨 후 이전 시간에 생성했던 프로젝트를 열고, 뷰포트 위주로 살펴보자.   
뷰포트에서는 카메라를 조작할 수 있다. 조작 방법에 대해 알아보자.

![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/screen-1.PNG)   

***

#### 🪐 기본 조작
뷰포트 화면에 탁자와 의자 두 개가 놓여 있고, 카메라는 이 액터들을 바라보고 있다. 해당 액터들을 컨트롤하기 위해서는 바라보는 카메라의 위치를 조작해야 우리가 원하는 대로 컨트롤할 수 있다.

> 💡   
**화면 전환(제자리)** : 마우스 우클릭 + 드래그   
**화면 전환(XY 이동)** : 마우스 우클릭 + W, A, S, D   
**화면 전환(Z 이동)** : 마우스 우클릭 + Q, E

![Alt Text](/assets/images/posts_img/engines/unreal/control/camera-speed.PNG)   

**카메라의 속도**는 뷰포트 우측 상단에서 변경할 수 있다.

***

#### 🪐 Object Focus
뷰포트를 움직이다 보면 컨트롤하고자 하는 액터가 화면에서 사라지는 때가 생긴다. 이 때 다시 액터를 화면의 중심으로 오게 하는 것을 **오브젝트 포커스(Object Focus)**라고 하는데, 두 가지 방법이 있다.   

> 1. **월드 아웃라이너 패널**에서 해당 액터를 찾아 **더블클릭** 한다.
2. **해당 액터를 선택한 상태**에서 **F키**를 누른다.

적응만 하면 아주 간단한 조작법이다.

***

### 🌱 액터의 이동, 회전, 크기 조작하기
이번에는 화면상에 있는 액터를 하나 선택해서 **이동, 회전, 크기 조절**을 해보자.

***

#### 🪐 이동(Move)
먼저 뷰포트의 우측 상단에 **트랜스폼(Transform) 툴** 중에 **무브 툴(Move Tool)**을 선택해보자. 단축키는 **W키**이다.

![Alt Text](/assets/images/posts_img/engines/unreal/control/move-tool.PNG)   

보통은 액터를 마우스 좌클릭으로 선택을 하면, 선택된 상태를 알려주기 위해 액터 주위로 노란색 선이 생기는 것을 볼 수 있고, **빨간색, 녹색, 파란색으로 이루어진 화살표**도 추가로 보이는 것을 확인할 수 있다. 이 화살표를 **기즈모(Gizmo)**라고 부른다.

![Alt Text](/assets/images/posts_img/engines/unreal/control/gizmo.PNG)   

기즈모가 보이는 상태에서 화살표에 마우스를 가져가면 노란색으로 변한다. 이 상태에서 좌클릭 후 드래그를 하면 액터가 화살표의 진행방향이나 그 반대 방향으로의 이동이 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/control/gizmo2.PNG)   
👆 마우스 오버했을 때

기즈모를 이용해서 액터를 이동시키는 방법도 있지만 **수치를 입력해서 이동**하는 방법도 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/control/detail.PNG)   

**Details** 패널 상단에 **Transform** 컴포넌트 안 **Location** 항목의 수치를 변경하면 액터 이동이 가능하다. X, Y, Z의 색과 화살표의 색은 같은 방향을 나타내므로 쉽게 알 수 있다.

***

#### 🪐 회전(Rotate)
뷰포트의 우측 상단에 **트랜스폼(Transform) 툴** 중에 **로테이트 툴(Rotate Tool)**을 선택해보자. 단축키는 **E키**이다.  

![Alt Text](/assets/images/posts_img/engines/unreal/control/rotate-tool.PNG)   

무브 툴과는 다르게 로테이트 툴을 선택한 후 보이는 기즈모는 _색이 있는 각도기 모양_ 이다.

![Alt Text](/assets/images/posts_img/engines/unreal/control/gizmo4.PNG)   

마찬가지로 기즈모를 좌클릭한 다음 드래그해서 액터의 각도를 변경시킬 수도 있고, 우측에 ``` Details 👉 Transform 👉 Rotation ``` 항목에서 수치를 변경하여 각도를 변경시킬 수도 있다.

***

#### 🪐 스케일(Scale)
뷰포트의 우측 상단에 **트랜스폼(Transform) 툴** 중에 **스케일 툴(Scale Tool)**을 선택해보자. 단축키는 **R키**이다.  

![Alt Text](/assets/images/posts_img/engines/unreal/control/scale-tool.PNG)   

역시 기즈모가 변경된다. 무브 툴과 비슷하지만, 끝이 _박스 모양_ 으로 생겼다.

![Alt Text](/assets/images/posts_img/engines/unreal/control/gizmo5.PNG)   

해당 툴은 **액터의 크기를 변경**시킨다. 원하는 값에 따라 선택된 축으로만 스케일이 조절되고, 원본 비율을 유지한 채로 크기만을 조절할 때는 가운데 하얀색 박스를 선택하고 조절하면 된다. 마찬가지로 ``` Details 👉 Transform 👉 Scale ``` 항목에서 수치로도 크기 조절이 가능하다. 다만 수치로 조절할 때는 다른 항목과 다르게 **배수**를 입력해야 한다. 원본의 기준이 1이고, 2는 2배, 0.5는 0.5배를 뜻한다.

***

#### 🪐 좌표계
좌표계는 기준점에 따라 월드와 로컬, 두 가지로 나눌 수 있다.   

지구본 모양의 **월드 좌표계**는 동서남북과 같은 **불변의 기준**이다. 고로 내가 보고 있는 기준으로 정해지기 때문에 기즈모의 방향은 늘 일정하다. 맵의 특정 위치에 액터를 이동시키고 배치할 때는 편리하지만, 액터의 방향을 유지한 채로 밀어 넣거나 객체의 정면을 기준으로 회전하기 쉽지 않아서 불편하다는 점이 있다.   

박스 모양의 **로컬 좌표계**는 **객체 스스로가 기준**이다. 객체를 기준으로 정해지기 때문에 기즈모는 액터의 이동, 회전에 따라 방향이 달라지게 된다.   

단 스케일 툴은 _로컬 좌표계_ 만 사용한다.

![Alt Text](/assets/images/posts_img/engines/unreal/control/cycles.PNG)   

뷰포트 우측 상단에 정렬되어 있는 아이콘을 클릭하여 좌표계를 **월드에서 로컬(객체)**, 다시 **로컬에서 월드**로 변경할 수 있다. 단축키는 액터를 선택한 후 ``` Ctrl+` ```를 입력하면 된다.

<img src="/assets/images/posts_img/engines/unreal/control/world.PNG" width="45%">
<img src="/assets/images/posts_img/engines/unreal/control/local.PNG" width="45%">   

> 위의 두 사진은 각각 **월드**와 **로컬 기준**의 기즈모를 나타낸다.

***

### 🌱 스냅(Snap)
이동, 회전, 크기 조정을 할 때 _딱딱 걸리는 것같이_ 움직인다면 스냅이 작동하고 있다고 볼 수 있다. 뷰포트 우측 상단에 차례대로 이동, 회전, 크기를 조절하는 아이콘이 배치되어 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/control/snap.PNG)   

왼쪽부터 이동, 회전, 크기를 나타내는 스냅이고, 각 스냅 설정을 통해서 원하는 값만큼 조절할 수 있다.   

![Alt Text](/assets/images/posts_img/engines/unreal/control/snap2.PNG)   

각 값은 해당 각도만큼 이동하거나 회전하라는 의미를 가지게 되고, 스냅이 필요 없다면 기능을 **해제**할 수도 있다. 해제하게되면 딱딱 맞아떨어지지 않아서 자유롭게 배치가 가능하나 정확한 각도로 배치하기는 어렵다는 단점이 있어서 상황에 따라 알맞게 사용하면 될 것 같다.

***

## 👻 글을 마치며
이번 시간엔 액터를 컨트롤 하는 법을 알아보았다. 생각보다 별 게 없어서 놀랐다. 앞으로 맵을 만들고 액터를 배치할 때마다 수없이 사용하게 될텐데 얼른 익혀야겠다. 단축키 생활화! 😎

***

_출처_   
_[저자 깃허브](https://github.com/araxrlab/lifeunreal)_   
<span style="font-size: 0.7em; color: gray;">이 포스팅은 _'인생 언리얼 교과서(성안당)'_ 를 바탕으로 쓰였습니다.</span>   