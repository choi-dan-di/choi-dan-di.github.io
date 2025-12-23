---
title: "[Unreal Engine] BP - 상속성(Inheritance)"
excerpt: "블루프린트에서 클래스의 상속 관계를 지정하는 방법에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, oop, inheritance]

permalink: /unreal/bp-inheritance/
 
toc: true
toc_sticky: true

date: 2022-12-14 13:47:38+0900
last_modified_at: 2022-12-14 13:47:40+0900
---

## 👻 상속성
블루프린트에서 클래스 간의 관계 중 상속 관계를 지정하는 방법에 대해 알아보자.

우선 부모 클래스와 자식 클래스를 만들어준다. 여기서는 ``` BP_Player ```가 부모 클래스이고 나머지는 모두 자식 클래스이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-inheritance/classes.PNG)   

그런 다음 자식 클래스의 블루프린트로 들어가 상단에 ``` Class Settings ```를 클릭해준 후, 우측에서 부모 클래스를 지정해주면 된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-inheritance/class-settings.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-inheritance/parent.PNG)   

미리 모든 클래스가 만들어져 있으면 위의 방식처럼 설정하면 되고, 새로운 자식 클래스를 생성할 땐 생성 시에 부모 클래스를 설정해주면 된다.

상속을 받은 자식 클래스는 부모 클래스가 가지고 있는 변수 혹은 함수를 그대로 물려받아 사용할 수 있게 되고, 이렇게 관계를 설정해두면 관련된 다수의 클래스를 한 번에 관리할 수 있게 되면서 중복된 코드가 합쳐진다는 장점이 있다.

***

### 🌱 활용
- 자식 클래스 내의 블루프린트에서 부모 클래스의 변수 혹은 함수 사용 가능

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-inheritance/child-bp.PNG)   

- 블루프린트 내에서 자식 클래스의 타입을 부모 클래스로 설정하여 사용 가능

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-inheritance/bp1.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-inheritance/bp2.PNG)   

***

## 👻 캐스팅
위의 활용 사례에서 보았듯 상속 관계가 지정된 클래스 간의 캐스팅(타입 변환)은 자연스레 이루어진다. 블루프린트에서 캐스팅이 어떻게 이루어지는지 알아보자.

위에서 설정한 자식 클래스 ``` BP_Knight ```에 변수 ``` Stamina ```를 하나 따로 추가해주고, ``` BP_Player ``` 타입으로 설정해 캐스팅이 자연스레 이루어지도록 해주었다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-inheritance/casting.PNG)   

위에서 ``` BP_Knight ```를 생성하여 ``` BP_Player ``` 타입의 변수인 **Player**에 세팅해주었다. 이렇게되면 간접적으로 캐스팅이 이루어지게 되고 ``` BP_Knight ```의 고유 변수인 ``` Stamina ```는 **사용할 수 없게된다.**

> - 부모 클래스 타입으로 받아 사용할 때 자식 클래스의 고유 변수를 사용할 수 없다고 해서 해당 데이터가 소실되는 것은 아니다.
- 자식 👉 부모 방향의 캐스팅은 간접적으로 가능하지만 부모 👉 자식 방향은 간접적으로 캐스팅이 불가능하다.

***

### 🌱 Cast To
``` Cast To ```를 사용하게 되면 직접적인 캐스팅이 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-inheritance/cast-to.PNG)   

> **Cast Failed** : 캐스팅 실패 시 실행될 부분

***

## 👻 글을 마치며
이번 시간에는 블루프린트에서 상속 관계를 설정하는 방법과 캐스팅에 대해 알아보았다. 이미 상속성에 대해 알고 있어서 이해가 빨랐고, C++ 보다 시각적으로 잘 보이니까 금방 익힐 수 있었던 것 같다. 캐스팅 노드도 캐스팅 실패시 진행되는 부분이 있어서 유용하게 잘 쓰일 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/oop/practice/BP_Inheritance)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   