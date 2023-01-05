---
title: "[Unreal Engine] BP - 인터페이스(Interface)"
excerpt: "블루프린트의 인터페이스에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, oop, interface]

permalink: /unreal/bp-interface/

toc: true
toc_sticky: true

date: 2023-01-05 18:34:59+0900
last_modified_at: 2023-01-05 18:35:03+0900
---

## 👻 인터페이스
**인터페이스(Interface)**는 함수 선언부만 모아둔 기본 설계도이다. 어떠한 기능이 있는지 우선적으로 적어놓은 다음, 인터페이스를 상속 받은 클래스가 해당 기능을 구현하여 입맛에 맞게 사용할 수 있도록 한다. 다중 상속과 비슷한 개념이지만 조금 더 안전하고 확실한 코드를 구현할 수 있게 해준다.

생성은 블루프린트 클래스와 동일하다. **Blueprints 👉 Blueprint Interface**를 클릭해 만들 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-interface/interface.PNG)   

만들어진 인터페이스를 더블 클릭하면 상세 페이지로 이동할 수 있고, 어떠한 함수를 정의할 것인지 **Add**버튼을 눌러 함수를 추가할 수 있다.

그런 다음, 인터페이스를 상속 받을 클래스의 상세 페이지로 들어가 인터페이스를 추가해주면 함수를 구현할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-interface/class-settings.PNG)   

상단의 **Class Settings**를 클릭한 다음, 우측 **Interface**에서 인터페이스를 관리할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-interface/add-interface.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-interface/add-interface2.PNG)   

인터페이스 내의 함수를 선언할 때, ``` Fly ``` 함수와 ``` Land ``` 함수를 만들어주었고, ``` Land ``` 함수에는 **Output**을 설정해주었다. 함수의 입출력이 설정되어 있으면 **함수**로, 아무것도 설정되어 있지 않으면 **이벤트**로 만들어진다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-interface/interfaces.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-interface/fly.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-interface/land.PNG)   

``` Fly ``` 함수는 입출력이 아무것도 설정되어 있지 않아서 **이벤트**로, ``` Land ``` 함수는 출력이 하나 설정되어 있어서 **함수**로 만들어진 것을 확인할 수 있다.

이렇게 만들어진 인터페이스 함수를 클래스 내에서 구현하고 레벨 블루프린트에 동일하게 적용하면 사용이 가능하다. 또한, 부모 클래스 타입의 변수를 만들어 자식 클래스를 받을 수 있는 것처럼, **인터페이스 타입**의 변수를 만들어 해당 인터페이스를 상속 받은 클래스를 동일하게 받을 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-interface/variables.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-interface/set-interface.PNG)   

***

## 👻 글을 마치며
이번 시간에는 인터페이스에 대해 알아보았다. 청사진과 비슷하다고 느꼈었는데, 애초에 블루프린트가 청사진이 아닌가..? 싶은 생각이 들기도 했다. 이해하는데 크게 어렵진 않았지만 적용을 해야되는 상황은 아직은 감이 잘 오질 않는 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/oop/BP_Interface)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   